#!/bin/bash

# Проверяем наличие аргумента
if [[ -z "$1" ]]; then
    echo "    Не указана целевая директория" >&2
    echo "    Формат: $0 /путь/к/директории" >&2
    exit 1
fi

# Инициализация переменных
src_path="$1"
parent_dir=$(dirname "$src_path")
dir_name=$(basename "$src_path")
current_time=$(date +"%Y%m%d-%H%M")
backup_dir="${parent_dir}/${dir_name}_backup_${current_time}"

# Проверка существования исходной директории
if [[ ! -d "$src_path" ]]; then
    echo "[X] Ошибка: директория '$src_path' не найдена" >&2
    exit 1
fi

# Создание директории для бэкапа
echo "[-] Создаю резервную копию в: $backup_dir"
if ! mkdir -p "$backup_dir"; then
    echo "[X] Не удалось создать директорию для бэкапа" >&2
    exit 1
fi

# Поиск и копирование файлов
total_files=0

while IFS= read -r -d '' file; do
    cp -v "$file" "$backup_dir" && ((total_files++))
done < <(find "$src_path" -type f \( -iname "*.jpg" -o -iname "*.jpeg" -o -iname "*.png" -o -iname "*.gif" -o -iname "*.heic" \) -print0)

# Проверка результатов
if (( total_files == 0 )); then
    echo "[!] Предупреждение: фотографии не обнаружены"
    rmdir "$backup_dir"
    exit 1
else
    echo "[✓] Успешно сохранено файлов: $total_files"
    echo "    Полный путь: $backup_dir"
fi

exit 0
