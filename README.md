Установка Arch Linux с BSPWM и настройка в стиле gh0stzk

Этот документ описывает пошаговую установку Arch Linux с оконным менеджером BSPWM и настройку окружения в стиле из репозитория gh0stzk.

Предварительные требования
Перед началом установки убедитесь, что у вас есть:
- Доступ в интернет.
- USB-накопитель с образом Arch Linux.
- Готовность использовать терминал и следовать инструкциям.

### Шаг 1: Подготовка и установка Arch Linux
1. Загрузите Arch Linux с USB:
Вставьте USB-накопитель и выберите его для загрузки в BIOS/UEFI.

### 2. Подключитесь к интернету:
```bash
iwctl
device list
station <устройство> scan
station <устройство> get-networks
station <устройство> connect <SSID>
exit
```
### 3. Синхронизируйте системные часы:
```bash
timedatectl set-ntp true
```
### 4. Создайте разделы на диске:
Используйте cfdisk или fdisk для создания разделов.

### 5. Форматируйте разделы и монтируйте их:
```bash
mkfs.ext4 /dev/sdX1
mkswap /dev/sdX2
swapon /dev/sdX2
mount /dev/sdX1 /mnt
```

### 6. Установите базовую систему:
```bash
pacstrap /mnt base linux linux-firmware
```

### 7. Настройте fstab:
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### 8. Войдите в новую систему:
```bash
arch-chroot /mnt
```

### 9. Настройте время:
```bash
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc
```

### 10. Настройте локализацию:
Отредактируйте /etc/locale.gen и раскомментируйте нужные локали, затем выполните:
```bash
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

### 11. Настройте сеть:
```bash
echo "myhostname" > /etc/hostname
echo "127.0.0.1   localhost" >> /etc/hosts
echo "::1         localhost" >> /etc/hosts
echo "127.0.1.1   myhostname.localdomain myhostname" >> /etc/hosts
```

### 12. Установите загрузчик:
```bash
pacman -S grub
grub-install --target=i386-pc /dev/sdX
grub-mkconfig -o /boot/grub/grub.cfg
```

### 13. Создайте пользователя:
```bash
useradd -mG wheel username
passwd username
```

### 14. Настройте sudo:
```bash
pacman -S sudo
EDITOR=nano visudo
```

Раскомментируйте строку:
```bash
%wheel ALL=(ALL) ALL
```
