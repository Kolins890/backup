Домашнее задание к занятию 2 "Резервное копирование" - Николай Чернов

Задание 1

Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup
Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
На проверку направить скриншот с командой и результатом ее выполнения

<img width="1360" height="682" alt="image" src="https://github.com/user-attachments/assets/93f1a454-fb3d-4c64-a054-3b7fde94e10a" />

<img width="1362" height="130" alt="image" src="https://github.com/user-attachments/assets/fc80e9f7-e386-4760-9ab6-bbf51c673c0b" />


Задание 2

Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
Резервная копия должна быть полностью зеркальной
Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
Резервная копия размещается локально, в директории /tmp/backup
На проверку направить файл crontab и скриншот с результатом работы утилиты.

Скрипт `backup_home.sh` выполняет ежедневное зеркальное копирование домашней директории.

```bash
#!/bin/bash

SOURCE_DIR="/home/kolins/"
BACKUP_DIR="/tmp/backup"
LOG_FILE="/var/log/backup.log"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

log_message() {
    echo "[$TIMESTAMP] $1" >> "$LOG_FILE"
}

log_message "Backup started"
rsync --archive --delete "$SOURCE_DIR" "$BACKUP_DIR"

if [ $? -eq 0 ]; then
    log_message "Backup completed successfully"
else
    log_message "Backup failed with error code $?"
fi
