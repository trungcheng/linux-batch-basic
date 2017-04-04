## Syntax

```sh
#!/bin/bash
# use predefined variables to access passed arguments
# echo arguments to the shell
# statement
echo "..."
if else
rsync 
...vân vân và vân vân

```

# Một số Shell có sẵn trên Linux:
- BASH ( Bourne-Again SHell ) phát triển bởi Brian Fox và Chet Ramey. Đây là Shell thông dụng nhất trên Linux.
- CSH (C SHell) phát triển bởi Bill Joy tại University of California (dành cho BSD). Sử dụng cấu trúc lệnh giống C, rất thân thiện cho các lập trình viên C trên linux.
- KSH (Korn SHell) phát triển bởi David Korn tại AT & T Bell Labs.
- TCSH bạn có thể gọ lệnh #man tcsh để xem thông tin.

# Giới thiệu
- Sử dụng với mục đích rút ngắn hay giảm bớt những tác vụ chúng ta hay dùng nhiều lần trên command-line, hay cần những tác vụ lặp đi lặp lại nhiều lần như crontab...v.v, hay đồng bộ hóa dữ liệu từ local lên server
- Lập trình shell-script cũng giống như những ngôn ngữ lập trình khác (echo, if else, for...)
- Tạo file đuôi .sh hoặc .bash đều ok

# Vài ví dụ cơ bản
- Các loại toán tử:
```sh
-lt <
-gt >
-le <=
-ge >=
-eq ==
-ne !=
```

- Backup thư mục đơn giản:
```sh
#!/bin/bash
tar -czf www.tar.gz /home/www​
```

- So sánh chuỗi:
```sh
#!/bin/bash
#Define chuoi 1
S1=”Chuoi thu nhat”
#Define chuoi 2
S2=”Chuoi thu hai”
if [ $S1 = $S2 ]
; then
echo “Hai chuoi bang nhau”
else
echo “Hai chuoi khong bang nhau”
fi​
```

- Start server A trên B khi A chết
```sh
#!/bin/bash
#neu dich vu tren server A chet thi start dich vu do tren Server B
#o day vi du la dich vu httpd
#host A da ssh ko hoi password bang host B va nguoc lai
#use "`<command shell in here>`" để thực thi một command
while [ 1 ]
do
status=`ps -ef | grep httpd | grep -v grep | wc -l`
if [ $status -gt 0 ]
then
sleep 60
else
ssh -i 192.168.1.3 “service httpd restart”
echo `date` | mail -s “service httpd down” root@localhost
break
fi
done
```

- ssh tự động
```sh
#!/bin/bash
for i in `seq 149 150`
do
ssh 192.168.75.$i ‘ssh-keygen -t rsa;\
cat /root/.ssh/id_rsa.pub’ >> /root/.ssh/authorized_keys
done
for i in `seq 149 150`
do
scp /root/.ssh/authorized_keys 192.168.75.$i:/root/.ssh/
done
```

- Backup MYSQL trên server Linux
```sh
#!/bin/bash
echo Starting Backup
mysqldump web_data > /backup/database/web_data_`date +%e-%m-%y`.sql
echo Backup Finished​
```

- Exit code:
```sh
#!/bin/bash
#Create new file and chuyen huong loi sang thu muc /dev/null
touch /root/hidden_diary 2> /dev/null
#$? la exit code - return 0 or 1 (0 - success, 1 - failed)
if [ $? -eq 0 ]
then
  echo "Yeahhh!!!"
  #return value
  exit 0
else
  echo "Oh no, could not create file..." >&2
  #return value
  exit 1
fi
```

- Auto deployment laravel project to server
```sh
#!/bin/bash
SERVER_IP="127.0.0.1"
SERVER_SITE_PATH="/var/www/example/dev/"
SERVER_USERNAME="ec2-user"

(>&2 echo "This script will deploy the application to staging. Please be mindful while doing this!")
(>&2 echo -n "Please confirm that you really want to deploy the current codebase to staging [y/N]: ")
read confirmation

if [[ $confirmation = 'y' || $confirmation = 'Y' ]]
    then
        (>&2 echo "The current codebase will now be deployed to the staging environment!")
    	# run composer
     	(>&2 echo "Attempting to run composer...")
     	APP_ENV="staging" composer install --no-dev
     	# fix rights to storage directory
     	ssh $SERVER_USERNAME@$SERVER_IP "sudo chown -R ec2-user:apache $SERVER_SITE_PATH/releases/$DIRECTORY_NAME"
     	# provide .env file
     	(>&2 echo "Attempting to make a symlink to the .env file in the project directory...")
     	ssh $SERVER_USERNAME@$SERVER_IP "ln -s $SERVER_SITE_PATH/.env $SERVER_SITE_PATH/releases/$DIRECTORY_NAME/.env"
     	# show current version
     	(>&2 echo "Attempting to show the current version...")
     	ssh $SERVER_USERNAME@$SERVER_IP "ls -lah $SERVER_SITE_PATH/current"
     	# run database migrations
     	(>&2 echo "Attempting to run database migrations...")
     	ssh $SERVER_USERNAME@$SERVER_IP "php $SERVER_SITE_PATH/releases/$DIRECTORY_NAME/artisan migrate --env=staging"
    else
	(>&2 echo "You chose not to run the database migrations...")
    fi
fi
```

- Kiểm tra số nguyên tố
```sh
#!/bin/bash
read -p “nhap so de kiem tra: ” a
i=2
while (( $i < $a ))
do
if (( $a%$i == 0 ));then
echo $a ko la so nguyen to
break
elif (( $a-1 == $i ));then
echo $a la so nguyen to
fi
let i+=1
done
```

- Chạy crontab 1 công việc (crontab -e)
```sh
#!/bin/sh
mkdir -p/root/test
cp /tmb/* /root/test
cd /root/test
taz -cvf test.tar/root/test/*
cp /root/test/test.tar/home/someuser/tmp
// Sau đó cho chạy định kỳ vào 4h sáng, tạo 1 file crontab với lệnh crontab -e rồi paste lệnh dưới vào
0 4 * * * /bin/sh/backup/backup.sh >/dev/null
# restart cron
etc/init.d/crond restart
```

- Đồng bộ dữ liệu với rsync
```sh
#!/bin/sh
# Copy dữ liệu từ local lên server
# Các option: --progress, --exclude, --include, --delete, --max-size
rsync -avzhe ssh backup.tar root@192.168.0.100:/backups/ --exclude example.txt
```
- Thực thi: ./tên-file-bash
- Nhớ chmod 755 cho nó
