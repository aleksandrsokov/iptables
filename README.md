IPtables  

Разворачиваем инфраструктуру (схема в файле схема.png):  
 список роутеров и их адресация:  
   1. **inetRouter** (Роутер смотрящий в интернет)  
      192.168.50.10/24 (для подключения по ssh)  
      192.168.255.1/30  - router-net  
      _обратный маршрут 192.168.0.0/16 через 192.168.255.2(centralRouter)_  
      
      так же реализован механизм knoking port. Настройки iptables в файле  templates/iptables_rules.ipv4  
      запуск скрипта knock.sh host 8881 7777 9991  


   1. **inetRouter2** (Роутер смотрящий в интернет)  
      192.168.50.17/24 (для подключения по ssh)  
      
      настроен проброс порта 8888 -> 192.168.50.12:80(centralServer). Настройки iptables в файле  templates/iptables_rules.ipv4
      


   3. **centralRouter** (центральный роутер)  
      192.168.255.2/30 - router-net  
      _маршрут 0.0.0.0/0 через 192.168.255.1 (inetRouter)_  
      192.168.0.1/28 - dir-net    
      192.168.0.33/28 - hw-net    
      192.168.0.65/26 - mgt-net    
      192.168.255.9/30 - office1-central  
      _обратный маршрут 192.168.2.0/24 через 192.168.255.10(office1Router)_  
      192.168.255.5/30 - office2-central  
      _обратный маршрут 192.168.1.0/24 через 192.168.255.6(office2Router)_  
      192.168.50.11/24 - (для подключения по ssh)  

   4. **office1Router** (роутер офина 1)  
      192.168.255.10/30 - dev1-net  
      _маршрут 0.0.0.0/0 через 192.168.255.9(centralRouter)_    
      192.168.2.1/26 - dev1-net  
      192.168.2.65/26 - test1-net  
      192.168.2.129/26 - managers-net     
      192.168.2.193/26 - office1-net  
      192.168.50.20/24 - (для подключения по ssh)  

   5. **office2Router** (роутер офина 1)  
      192.168.255.6/30 - dev1-net  
      _маршрут 0.0.0.0/0 через 192.168.255.5(centralRouter)_    
      192.168.1.1/26 - dev2-net  
      192.168.1.65/26 - test2-net    
      192.168.1.193/26 - office12-net  
      192.168.50.30/24 - (для подключения по ssh)  

   6. **office1Server** (Сервер офиса 1)  
      192.168.2.130/26 - test2-net  
      _маршрут 0.0.0.0/0 через 192.168.2.129(office1Router)_  
      192.168.50.31/24 - (для подключения по ssh)  

   7. **office2Server** (Сервер офиса 1)  
      192.168.1.130/26 - managers-net  
      _маршрут 0.0.0.0/0 через 192.168.1.129(office2Router)_  
      192.168.50.21/24 - (для подключения по ssh)  

   8. **centralServer** (Сервер щентральный)   
      192.168.50.12/24 - (для подключения по ssh)  
      Установлен Nginx. слушает порт 80