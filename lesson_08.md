# Дополнительный материал
1) [Сети - введение](https://youtu.be/3zCQGen6VpA)
2) [Сети - базовая настройка](https://youtu.be/2OljPJn3H1Q)
3) [Сети - ssh](https://youtu.be/abmnnC4kZnI)
4) [Сети - DNS](https://youtu.be/Y_PP4gwST7k)
5) [Сети - iptables](https://youtu.be/Bd9itUD1_aU)

# Задания
## dhcp
### cheet sheet
```bash
# посмотреть адреса/интерфейсы
ip -o a
ip link show
```

```bash
# настройка сетевых интерфейсов
sudo nano /etc/netplan/01-network-manager-all.yaml
```

```yaml
  network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: yes
    enp0s8:
      dhcp4: no
      address: 172.16.1.5/24
```

```bash
# применение настроек
sudo netplan generate
sudo netplan apply
```

```bash
# установка dhcp сервера
sudo apt install isc-dhcp-server
```

```bash
# настройка сервера
sudo nano /etc/dhcp/dhcpd.conf
```

```ini
subnet 172.16.1.0 netmask 255.255.255.0 {
  range 172.16.1.10 172.16.1.20;
}
```

```bash
sudo nano /etc/default/isc-dhcp-server
```

```ini
INTERFACESv4="enp0s8" # тут ваш интерфейс
```

```bash
sudo systemctl restart isc-dhcp-server.service
sudo systemctl status isc-dhcp-server.service
```

Далее на client меняем в `/etc/netplan/01-network-manager-all.yaml` для enp0s8 параметр dhcp на yes и убираем address; рестарутем сеть и проверяем с помощью ip

### задание
1) Организовать две ВМ на базе Linux с именами server и client с двумя сетевыми интерфейсами (лучше VirtualBox, Vmware, etc, а не cloud)
- первый с типом Bridged для связи с хостом и внешним миром
- второй с типом Internal для внутренней сети
2) Задать обеим ВМ статические ip адреса из сети `172.16.1.0/24`
3) Проверить сетевую свзяность между ВМ по заданным статическим внутренним адресам
4) На ВМ server развернуть dhcp-сервер, который будет раздавать ip адреса из пула `172.16.1.10-172.16.1.20`
5) На ВМ client настроить внутренний сетевой интерфейс на получение ip от dhcp сервера; проверить сетевую связность между ВМ

## iptables
### cheet sheet
```bash
# сброс правил
iptables -F && iptables -X && iptables -P INPUT ACCEPT
# Сохранить правила
iptables-save > /etc/iptables/rules.v4
# если что-то пошло не так
iptables -P INPUT ACCEPT && iptables -F
```
### задание
1) Запустить три ВМ - одна server, вторая клиент (можно взять из прошлой ДЗ) и третья client-2
2) Установить пакет iptables на server, если еще не установлен
3) Вывести все правила для всех таблиц и цепочек
4) Вывести правила только для таблицы filter
5) Разрешить ping (icmp) от машины client и запретить от всех остальных
6) Разрешить ssh из подсети 192.168.1.0/24 и запретить для всех остальных; проверить ssh с машины client и с машины client-2
7) Установить на server postgres если еще не установлен и запретить внешний доступ на него, разрешив подключаться только локально; проверить с любой машины client
8) Создать правило для логирования запросов к портам
	1) ssh
	2) http
	3) postgres
9) Ограничить количество подключений ssh тремя в минуту
