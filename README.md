# Simple Debian MPV Player 📺

[English](#english) | [Русский](#русский)

---

## English

A lightweight, fully automated solution to turn any PC into a dedicated video player. This project covers the entire lifecycle: from OS installation via PXE to a configured media player using Ansible.

## Key Features
* **Zero-touch OS Install:** Debian installation via PXE + Preseed (no manual partitioning or user creation).
* **Minimalist:** No heavy desktop environments. Just a bare-bones Debian with LightDM, LXDE-core, and MPV.
* **Speed:** Optimized Ansible playbooks that skip recommended packages and non-interactive setup.
* **Production Ready:** Auto-login and auto-start video script included.

## Project Structure
* `/pxe` — Debian Preseed configuration for automated netboot.
* `/ansible` — Playbooks and inventory to configure the video player.

## Quick Start

### 1. OS Installation (PXE)
1. Place the `pxe/preseed.cfg` on your web server.
2. Boot your target machine via PXE (netboot.xyz or similar).
3. Point the installer to your preseed URL:
   `auto=true priority=critical preseed/url=http://your-server/preseed.cfg`

### 2. Software Configuration (Ansible)
1. Edit `ansible/inventory.ini` and add your player's IP:
```ini
   [players]
   192.168.1.10
   192.168.1.12
   192.168.1.15
```

2. Edit `ansible/preseed.config` and replace `YOUR_PASS_HERE` with password which you will use to log in
  
3. Run the playbook:
```bash
   ansible-playbook -i inventory.ini master_playbook.yml -k -K
```

## Requirements
* Target machine with Debian installed (using provided preseed).
* Ansible installed on your control node (or running via Docker).
* Network access between the control node and players.

---

## Русский

Легкое и полностью автоматизированное решение, позволяющее превратить любой ПК в автоматизированный видеоплеер. Этот проект охватывает весь цикл: от установки ОС через PXE до настройки медиаплеера с помощью Ansible.

## Основные особенности
* **Автоматическая установка ОС**: установка Debian через PXE + Preseed (без ручного разбиения диска и создания учетной записи пользователя).
* **Минимализм**: никаких тяжелых рабочих сред. Только чистый Debian с LightDM, LXDE-core и MPV.
* **Скорость**: оптимизированные плейбуки Ansible, которые пропускают стандартные пакеты и неинтерактивную настройку.
* **Готовность к автоматизированному использованию**: включены скрипты для автоматического входа в систему и автоматического запуска видео.

## Структура проекта
* `/pxe` — конфигурация Debian Preseed для автоматической загрузки по сети.
* `/ansible` — плейбук и файл инвентаря для настройки видеоплееров.

## Быстрый старт

### 1. Установка ОС (PXE)
1. Разместите файл `pxe/preseed.cfg` на вашем веб-сервере.
2. Загрузите целевую машину через PXE (netboot.xyz или аналогичный сервис).
3. Укажите установщику URL-адрес вашего файла preseed:
   `auto=true priority=critical preseed/url=http://your-server/preseed.cfg`

### 2. Настройка программного обеспечения (Ansible)
1. Отредактируйте файл ansible/inventory.ini и добавьте IP-адреса ваших плееров:
```ini
   [players]
   192.168.1.10
   192.168.1.12
   192.168.1.15
```

2. Отредактируйте файл `ansible/preseed.config` и замените `YOUR_PASS_HERE` на пароль, который вы будете использовать для входа
  
3. Запустите плейбук:
```bash
   ansible-playbook -i inventory.ini master_playbook.yml -k -K
```

## Требования
* Целевой компьютер с установленной Debian (с использованием предоставленного preseed файла).
* Ansible, установленный на вашем сервере (или запущенный через Docker).
* Сетевой доступ между сервером и плеерами.
