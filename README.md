# systemd
Домашнее задание:
1. Написать service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова (файл лога и ключевое слово должны задаваться в /etc/sysconfig).

2. Из репозитория epel установить spawn-fcgi и переписать init-скрипт на unit-файл (имя service должно называться так же: spawn-fcgi).

3. Дополнить unit-файл httpd (он же apache) возможностью запустить несколько инстансов сервера с разными конфигурационными файлами
Все необходимые файлы находятся в папке httpd

Выполнение:
1. В папке files созданы все необходимые файлы, копируются во время vagrant up.  
	sudo cp /vagrant_files/watchlog /etc/sysconfig/watchlog  
	sudo cp /vagrant_files/watchlog.sh /opt/watchlog.sh 
	sudo cp /vagrant_files/watchlog.log /var/log/watchlog.log   
	sudo cp /vagrant_files/watchlog.service /usr/lib/systemd/system/watchlog.service    
	sudo cp /vagrant_files/watchlog.timer /usr/lib/systemd/system/watchlog.timer  

    Для запуска службы выполняется:     
     `sudo systemctl daemon-reload  
      sudo systemctl enable watchlog.timer   
      sudo systemctl start watchlog.timer`  

2.

3.

