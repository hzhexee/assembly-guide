# Как запускать и собирать ASM-задания на Linux (WSL) x86/x86_64

Этот гайд предназначен для тех, кто хочет запускать и компилировать ассемблерные задания, написанные для Linux x86/x86_64, используя WSL (Windows Subsystem for Linux).

## Шаг 1: Установка WSL

1. Откройте PowerShell от имени администратора.
2. Введите следующую команду для установки WSL и дистрибутива Ubuntu:

```powershell
wsl --install
```

3. Если WSL уже установлен, убедитесь, что используется WSL 2:

```powershell
wsl --set-default-version 2
```

1. Перезагрузите ПК.
2. Запустите Ubuntu из меню Пуск и дождитесь начальной настройки.

## Шаг 2: Установка необходимых инструментов

В терминале Ubuntu установите следующие пакеты:

```bash
sudo apt update && sudo apt install -y build-essential nasm git libc6-i386 gcc-multilib
```

**Объяснение:**
- **build-essential** — компиляторы (gcc, ld) и make.
- **nasm** — Netwide Assembler для сборки .asm файлов.
- **git** — для клонирования репозитория.

Также могут понадобиться следующие пакеты:

```bash
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install libc6:i386
```

## Шаг 3: Клонирование репозитория с заданиями

Рекомендуется клонировать репозиторий в домашнюю директорию пользователя для удобства доступа:

```bash
cd ~/
git clone https://github.com/твое_имя_или_организация/название-репозитория.git
cd название-репозитория
```

Здесь и далее предположим, что каждая папка — это отдельное задание с .asm и/или Makefile.

## Шаг 4: Сборка и запуск заданий

### Ручная сборка

Для 32-битных программ:
```bash
nasm -f elf32 file.asm
ld -m elf_i386 -o out file.o   # ИЛИ gcc -m32 file.o -o file
./out
```

Если программа 64-битная:
```bash
nasm -f elf64 file.asm
ld -o out file.o
./out
```

> ⚠️ **Убедитесь, что архитектура кода (32/64-бит) совпадает с системой!** Для запуска 32-битных программ на 64-битной системе могут потребоваться дополнительные библиотеки

## Шаг 5: Работа с WSL через VSCode

Visual Studio Code предоставляет отличную интеграцию с WSL, что позволяет удобно редактировать файлы и использовать терминал прямо из IDE.

### Настройка VSCode для работы с WSL:

1. Установите VSCode, если он еще не установлен: https://code.visualstudio.com/
2. Установите расширение "Remote - WSL":
   - Откройте VSCode
   - Перейдите в раздел Extensions (Ctrl+Shift+X)
   - Найдите и установите "Remote - WSL" от Microsoft

### Открытие проекта из WSL:

1. **Метод 1**: Через командную строку WSL:
   ```bash
   cd ~/название-репозитория
   code .
   ```
   Это откроет текущую директорию в VSCode с подключением к WSL.

2. **Метод 2**: Через VSCode:
   - Нажмите на синюю кнопку в левом нижнем углу VSCode
   - Выберите "New WSL Window"
   - В открывшемся окне VSCode используйте меню File > Open Folder для навигации к вашему проекту

### Преимущества работы с VSCode и WSL:

- Интегрированный терминал WSL (Terminal > New Terminal)
- Отладка приложений прямо из VSCode
- Интеллектуальные подсказки для сборки и запуска
- Возможность установки расширений специально для работы в WSL

### Полезные расширения для работы с ассемблером:

- "x86 and x86_64 Assembly" - подсветка синтаксиса для ассемблера
- "Assembly Debug" - помощь с отладкой ассемблерных программ
- "Better NASM" - улучшенная поддержка NASM
