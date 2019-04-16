# nomad
Для развертывания Nomad на Ubuntu старше версии 16.0 :

- установить libvirt
- установить VirtualBox
  [How to Install Oracle VirtualBox On Ubuntu 18.04 LTS Headless Server](https://www.ostechnix.com/install-oracle-virtualbox-ubuntu-16-04-headless-server/)
  
    Добавить официальный репозиторий Oracle VirtualBox. Редактируем файл /etc/apt/sources.list:
    ```
    $ sudo nano /etc/apt/sources.list
    ```
    В конец файла добавить строку (для Ubuntu версии 18.04 *Bionic*):
    ```
    deb http://download.virtualbox.org/virtualbox/debian bionic contrib
    ```
    Если у вас другая версия Ubuntu - замените *bionic* на имя дистрибутива своей версии Ubuntu: *xenial*, *vivid*, *utopic*, *trusty*, *raring*, *quantal*, *precise*, *lucid*, *jessie*, *wheezy*, или *squeeze*
    
- развернуть Vagrant
