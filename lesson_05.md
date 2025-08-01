# Шапргалка по работе с LVM
```bash
# Установка lvm, если еще не установлен
apt install lvm2
# Просмотр текущих LVM-объектов (PV/VG/LV)
pvs
pvdisplay
vgs
vgdisplay
lvs
lvdisplay
# Создание физических томов
pvcreate /dev/vdb1
pvcreate /dev/vdb2
pvs
# Создание группы томов
vgcreate databases /dev/vdb1 /dev/vdb2
vgs
# Создание логического тома
lvcreate -n mysql -L1G databases
lvs
mkfs.ext4 /dev/mapper/databases-mysql
mkdir /opt/mysql
blkid
vim /etc/fstab
mount -a
df -h

lvextend -L+1G /dev/databases/mysql
lvextend -l +100%FREE /dev/databases/mysql
# или указать конечный размер
lvextend -L2G /dev/databases/mysql
# Расширение ФС
resize2fs /dev/databases/mysql
df -Th
```
# Задания
## Часть 1
1) Добавить два дополнительных диска к ВМ
2) Установить на ВМ postgresql, убедиться что он запущен
3) Выяснить, в какой директории postgres хранит данные
```bash
sudo su - postgres
psql
```

```sql
SHOW data_directory;
```
4) На одном новом диске создать 2 раздела (обычных, с помощью fdisk)
5) Второй диск полностью отдать под LVM:
- сделать из него PV
- из PV сделать VG
- из VG отрезать раздел под данные postgresql
6) Смонтировать созданный раздел в директорию, которую нашли в задании 3 (**не забудьте сделать бэкап директории**); проверить что postgres не сломался и работает
7) Создать из двух разделов на оставшемся диске два PV
8) Добавить их в сущеуствующую VG
9) Увеличить размер LV под данными postgres

## Часть 2
1) Установить mongodb
2) Выяснить, где находится директория с данными командой ```grep -i dbPath /etc/mongod.conf```
3) Подключить еще один дополнительный диск к ВМ
4) Сделать из нового диска PV, из PV - VG
5) Создать Logical Volume mongo-data и вынести директорию из п.6 на него (**не забудьте сделать бэкап директории**)
6) Убедиться, что все прошло корректно (сервис mongodb запущен, можно подключиться к БД)
