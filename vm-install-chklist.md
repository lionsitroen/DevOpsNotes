#Удалить пользователя user
userdel -r user

# сменить пароль root
passwd root

# установить ключ
mkdir -p /root/.ssh && echo '${pub-key}' >> .ssh/authorized_keys

# Установить необходимый софт
apt update && apt upgrade -y && apt install sudo dnsutils net-tools netcat vim mc curl htop iftop tcpdump tree fail2ban apt-transport-https gnupg2 tmux git pkg-config -y

# Настроить vim
echo "syntax on" >> .vimrc && echo "colorscheme elflord" >> .vimrc

apt install open-vm-tools -y

# отключить ip v6
в nano /etc/sysctl.conf
добавить:
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
перечитать конфиг: sysctl -p

# Cелать Aliasы
echo "alias ll='ls -lah --group-directories-first'" >> .bashrc

# Раскрасим приглашение (https://losst.ru/tsveta-terminala-linux)
vi .bashrc
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h:\w\$\[\033[00m\] '

# настроить часовой пояс
timedatectl status # посмотреть текущее время
timedatectl list-timezones #список доступных TZ
timedatectl set-timezone Europe/Moscow

# настроить NTP
vi /etc/systemd/timesyncd.conf
NTP=ваш NTP сервер
systemctl restart systemd-timesyncd
systemctl status systemd-timesyncd 
# Состояние синхронизации времени можно проверить утилитой timedatectl: 
timedatectl status
timedatectl timesync-status

# Настроить английскую локаль (настройки применятся при следущем входе)
vi /etc/locale.gen
locale-gen
# или 
dpkg-reconfigure locales


# Если ВМ получена копированием
# переименовать 
vi /etc/hostname
vi /etc/hosts
# Удалить\проверить старые ключи 
vi .ssh/authorized_keys

# Сделать копию ВМ
# Сделать снапшот

# изменить имя хоста
hostnamectl set-hostname host_name
vi /etc/hosts
hostnamectl

# Если нужен пользовател с sudo
adduser <username>
# ввсети енобходимые данные

#Добавить sudo если надо (https://www.8host.com/blog/redaktirovanie-fajla-sudoers-v-ubuntu-i-centos/)
visudo 
<username>    ALL=(ALL:ALL) ALL

# установка docker
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# установка docker-compose
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
# Settings for Fail2ban

######################
## Настройка fail2ban

vi /etc/fail2ban/jail.conf

# "bantime" is the number of seconds that a host is banned.
bantime  = 3h

# A host is banned if it has generated "maxretry" during the last "findtime"
# seconds.
findtime  = 31m

# "maxretry" is the number of failures before a host get banned.
maxretry = 3

service fail2ban restart

#################################
## установка zabbix агента v2 debian 10
### https://www.zabbix.com/download

# Install 
wget https://repo.zabbix.com/zabbix/5.4/debian/pool/main/z/zabbix-release/zabbix-release_5.4-1+debian10_all.deb && \
dpkg -i zabbix-release_5.4-1+debian10_all.deb && \
apt update && apt install -y zabbix-agent2 

# Check config
vi /etc/zabbix/zabbix_agent2.conf

LogFileSize=1
Server=<DNS or IP Zabbix Server>
ServerActive=<DNS or IP Zabbix Server>
Hostname=<local Hostame> # должен совпадать с именем на сервре, если не определено берется из HostnameItem (system.hostname)

service zabbix-agent2 restart
  
# полезная информация
  Перманантная настройка DNS  https://www.linuxfordevices.com/tutorials/linux/change-dns-on-linux
