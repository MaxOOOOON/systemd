# systemd
Домашнее задание:
1. Написать service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова (файл лога и ключевое слово должны задаваться в /etc/sysconfig).

2. Из репозитория epel установить spawn-fcgi и переписать init-скрипт на unit-файл (имя service должно называться так же: spawn-fcgi).

3. Дополнить unit-файл httpd (он же apache) возможностью запустить несколько инстансов сервера с разными конфигурационными файлами
Все необходимые файлы находятся в папке httpd

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

3.




