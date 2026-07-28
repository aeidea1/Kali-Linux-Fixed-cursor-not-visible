# Курсор мыши не отображается в Kali Linux 2026.1 (VMware Workstation)

## Симптомы
- Kali Linux в VMware Workstation, X11 сессия
- Курсор мыши физически не виден на экране гостевой ОС
- Мышь при этом работает (клики проходят, `xinput` видит устройство)
- `open-vm-tools` установлен и запущен нормально

## Причина
Драйвер `modesetting` (используется вместо устаревшего `xf86-video-vmware`,
которого больше нет в репозиториях) в связке с ядерным модулем `vmwgfx`
не поддерживает аппаратный курсор (hardware cursor) через KMS.

## Диагностика
```bash
# Проверить, что используется драйвер modesetting
grep -i "modesetting\|driver for screen" /var/log/Xorg.0.log

# Убедиться, что мышь определяется корректно
grep -i "vmware\|mouse\|pointer" /var/log/Xorg.0.log
```

## Решение
Принудительно включить программный курсор (SWcursor):

```bash
sudo mkdir -p /etc/X11/xorg.conf.d
sudo cp 20-vmware-cursor.conf /etc/X11/xorg.conf.d/
sudo reboot
```

## Проверка
```bash
grep -i "swcursor\|cursor" /var/log/Xorg.0.log
```

## Окружение
- Kali Linux 2026.1 (rolling)
- VMware Workstation
- X11 (Xfce)
