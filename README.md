# zfsotus

# создаем четыре pool 
zpool create otus1 mirror /dev/sdb /dev/sdc
zpool create otus2 mirror /dev/sdd /dev/sde
zpool create otus3 mirror /dev/sdf /dev/sdg
zpool create otus4 mirror /dev/sdh /dev/sdi

#смотрим информацию о пулах 
zpool list
zpool status

# создаем разные алгоритмы сжатия lzjb lz4 gzip zle 
zfs set compression=lzjb otus1
zfs set compression=lz4 otus2
zfs set compression=gzip-9 otus3
zfs set compression=zle otus4

# смотрим что получилось
zfs get all | grep compression

# скачиваем один файл на все пулы
for i in {1..4}; do wget -P /otus$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done

# проверяем что все скачалось
ls -l /otus*

# gzip-9 самый эффективный по сжатию
zfs get all | grep compressratio | grep -v ref

# скачиваем архив
wget -O archive.tar.gz --no-check-certificate https://drive.google.com/u/0/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&export=download

# распаковка
tar -xzvf archive.tar.gz

# импорт данного пула и проверка статуса 
zpool import -d zpoolexport/ otus
zpool status

# просмотр информации о пуле
zpool get all otus
zfs get available otus
zfs get readonly otus
zfs get recordsize otus
zfs get compression otus
zfs get checksum otus

# скачиваем файл
wget -O otus_task2.file --no-check-certificate https://drive.google.com/u/0/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&export=download

# восстановим файловую систему из снапшота
zfs receive otus/test@today < otus_task2.file

# ищем файл и смотрим содержимое
find /otus/test -name "secret_message"
https://github.com/sindresorhus/awesome

