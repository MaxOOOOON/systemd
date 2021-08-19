# systemd
Домашнее задание:
1. Написать service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова (файл лога и ключевое слово должны задаваться в /etc/sysconfig).

2. Из репозитория epel установить spawn-fcgi и переписать init-скрипт на unit-файл (имя service должно называться так же: spawn-fcgi).

3. Дополнить unit-файл httpd (он же apache) возможностью запустить несколько инстансов сервера с разными конфигурационными файлами

Выполнение:
1. В папке files созданы все необходимые файлы, копируются во время vagrant up:	  

        sudo cp /vagrant_files/watchlog /etc/sysconfig/watchlog  
        sudo cp /vagrant_files/watchlog.sh /opt/watchlog.sh 	
        sudo cp /vagrant_files/watchlog.log /var/log/watchlog.log   
        sudo cp /vagrant_files/watchlog.service /usr/lib/systemd/system/watchlog.service    
        sudo cp /vagrant_files/watchlog.timer /usr/lib/systemd/system/watchlog.timer
        
    Ключевое слово прописывается в файле /etc/sysconfig/watchlog
    
    Для корректного выполнения скрипта раз в 30 сек необходимо прописать в разделе Timer
        
        Unit=watchlog.service
        OnUnitActiveSec=30
        AccuracySec=1us

    Для запуска службы выполняется:   
    
        sudo systemctl daemon-reload  
        sudo systemctl enable watchlog.timer   
        sudo systemctl start watchlog.timer
        
    Для просмотра списка таймеров можно выполнить:  
        
        systemctl list-timers
      ![изображение](https://user-images.githubusercontent.com/36797330/130148540-46f6006c-ba06-48bc-9021-9c33be58efac.png)

2. Для установки spawn-fcgi необходимо выполнить:
 
        dnf install epel-release
        dnf install -y spawn-fcgi
        dnf install -y php php-cli mod_fcgid httpd
        
   Прописать в файле /etc/sysconfig/spawn-fcgi
   
        SOCKET=/var/run/php-fcgi.sock
        OPTIONS="-u apache -g apache -s $SOCKET -S -M 0600 -C 32 -F 1 -P /var/run/spawn-fcgi.pid -- /usr/bin/php-cgi"

   И создать службу /etc/systemd/system/spawn-fcgi.service 
   
        [Unit]
        Description=Spawn-fcgi startup service by Otus
        After=network.target
        [Service]
        Type=simple
        PIDFile=/var/run/spawn-fcgi.pid
        EnvironmentFile=/etc/sysconfig/spawn-fcgi
        ExecStart=/usr/bin/spawn-fcgi -n $OPTIONS
        KillMode=process
        [Install]
        WantedBy=multi-user.target
   
   
3. Все необходимые файлы находятся в папке httpd  
      
      Конфиги first.conf и second.conf нужно прописать в /etc/httpd/conf    
      В /etc/sysconfig/ скопировать 2 файла окружения https://github.com/MaxOOOOON/systemd/blob/main/httpd/httpd-first и https://github.com/MaxOOOOON/systemd/blob/main/httpd/httpd-second    
      В /etc/systemd/system/ создать файл службы https://github.com/MaxOOOOON/systemd/blob/main/httpd/httpd%40.service
      
      и запустить 
      
        systemctl start httpd@first
        systemctl start httpd@second
       
      ![изображение](https://user-images.githubusercontent.com/36797330/130152145-99f219ea-3611-4de0-bf82-52c9a911a1d3.png)

      ![изображение](https://user-images.githubusercontent.com/36797330/130152167-2a294ad2-3a04-4f7c-9cb6-61ae7d09e96a.png)





