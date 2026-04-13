# Simple Debian MPV Player 📺

[English](#english) | [Русский](#русский)

---

## English

A lightweight, fully automated solution to turn any PC into a dedicated video player. This project covers the entire lifecycle: from OS installation via PXE to a configured media player using Ansible.

## Key Features
* **Zero-touch OS Install:** Debian installation via PXE + Preseed (no manual partitioning or user creation).
* **Minimalist:** Bare-bones Debian with LightDM, LXDE-core, and MPV.
* **Speed:** Optimized Ansible playbooks that skip recommended packages and non-interactive setup.
* **Production Ready:** Auto-login, auto-start video script and auto shutdown at 5 PM service with timer included.

## Project Structure
* `/pxe` — Debian Preseed configuration for automated netboot.
* `/ansible` — Playbooks and inventory to configure the video player.

## Quick Start

### 1. OS Installation (PXE)
1. Edit `pxe/preseed.config` and replace `YOUR_PASS_HERE` with password which you will use to log in
2. Place the `pxe/preseed.cfg` on your web server.
3. Boot your target machine via PXE (netboot.xyz or similar).
4. Point the installer to your preseed URL:
   `auto=true priority=critical preseed/url=http://your-server/preseed.cfg`

### 2. Software Configuration (Ansible)
1. Edit `ansible/inventory.ini` and add your player's IP:
```ini
   [players]
   192.168.1.10
   192.168.1.12
   192.168.1.15
```

2. Edit timer and service settings for yourself in `ansible/setup_playbook.yml`:
```yaml
- name: system service for shutdown
      copy:
        dest: /etc/systemd/system/shutdown-at-17.service #same name with timer
        content: |
          [Unit]
          Description=Shutdown Computer at 17:00
          [Service]
          Type=oneshot
          ExecStart=/usr/sbin/shutdown -h now

    - name: system timer for shutdown
      copy:
        dest: /etc/systemd/system/shutdown-at-17.timer #same name with service
        content: |
          [Unit]
          Description=Timer to shutdown computer at 17:00
          [Timer]
          OnCalendar=*-*-* 17:00 #time stamp for shutdown
          AccuracySec=1min
          [Install]
          WantedBy=timers.target

    - name: start and enable timer
      systemd:
        name: shutdown-at-17.timer #same name as above
        enabled: yes
        state: started
        daemon_reload: yes
```
  
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
* **Минимализм**: чистый Debian с LightDM, LXDE-core и MPV.
* **Скорость**: оптимизированные плейбуки Ansible, которые пропускают стандартные пакеты и неинтерактивную настройку.
* **Готовность к автоматизированному использованию**: включены скрипты для автоматического входа в систему, автоматического запуска видео и автоматического выключения сервера в 17:00.

## Структура проекта
* `/pxe` — конфигурация Debian Preseed для автоматической загрузки по сети.
* `/ansible` — плейбук и файл инвентаря для настройки видеоплееров.

## Быстрый старт

### 1. Установка ОС (PXE)
1. Отредактируйте файл `pxe/preseed.config` и замените `YOUR_PASS_HERE` на пароль, который вы будете использовать для входа
2. Разместите файл `pxe/preseed.cfg` на вашем веб-сервере.
3. Загрузите целевую машину через PXE (netboot.xyz или аналогичный сервис).
4. Укажите установщику URL-адрес вашего файла preseed:
   `auto=true priority=critical preseed/url=http://your-server/preseed.cfg`

### 2. Настройка программного обеспечения (Ansible)
1. Отредактируйте файл `ansible/inventory.ini` и добавьте IP-адреса ваших плееров:
```ini
   [players]
   192.168.1.10
   192.168.1.12
   192.168.1.15
```

2. Отредактируйте настройки таймеров и сервисов под себя в файле `ansible/setup_playbook.yml`:
```yaml
- name: system service for shutdown
      copy:
        dest: /etc/systemd/system/shutdown-at-17.service #такое же имя как у таймера
        content: |
          [Unit]
          Description=Shutdown Computer at 17:00
          [Service]
          Type=oneshot
          ExecStart=/usr/sbin/shutdown -h now

    - name: system timer for shutdown
      copy:
        dest: /etc/systemd/system/shutdown-at-17.timer #такое же имя как у сервиса
        content: |
          [Unit]
          Description=Timer to shutdown computer at 17:00
          [Timer]
          OnCalendar=*-*-* 17:00 #временная метка для выключения
          AccuracySec=1min
          [Install]
          WantedBy=timers.target

    - name: start and enable timer
      systemd:
        name: shutdown-at-17.timer #такое же имя как и выше
        enabled: yes
        state: started
        daemon_reload: yes
```
  
3. Запустите плейбук:
```bash
   ansible-playbook -i inventory.ini setup_playbook.yml -k -K
```

## Требования
* Целевой компьютер с установленной Debian (с использованием предоставленного preseed файла).
* Ansible, установленный на вашем сервере (или запущенный через Docker).
* Сетевой доступ между сервером и плеерами.
