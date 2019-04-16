# nomad
Для развертывания Nomad на Ubuntu старше версии 16.0 :

- установить libvirt
- **установить VirtualBox**, оригинальный гайд:
  [How to Install Oracle VirtualBox On Ubuntu 18.04 LTS Headless Server](https://www.ostechnix.com/install-oracle-virtualbox-ubuntu-16-04-headless-server/)
  
    Добавим официальный репозиторий Oracle VirtualBox. Редактируем файл /etc/apt/sources.list:
    ```
    $ sudo nano /etc/apt/sources.list
    ```
    В конец файла нужно добавить строку (для Ubuntu версии 18.04 *Bionic*):
    ```
    deb http://download.virtualbox.org/virtualbox/debian bionic contrib
    ```
    Если у вас другая версия Ubuntu - замените *bionic* на имя дистрибутива своей версии Ubuntu: 
    - 16.04 *xenial*, 
    - 15.04 *vivid*, 
    - 14.10 *utopic*, 
    - 14.04 *trusty*, 
    - 13.04 *raring*, 
    - 12.10 *quantal*, 
    - 12.04 *precise*, 
    - 10.04 *lucid*
    
    После этого добавьте публичный ключ репозитория: 
    ```
    $ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
    ```
    Обновим репозиторий:
    ```
    $ sudo apt update
    ```
    Устанавливаем последнюю версию VirtualBox (на данный момент это 5.2)
    ```
    $ sudo apt install virtualbox-5.2
    ```
    После этого нужно добавить своего пользователя в группу `vboxusers`:
    ```
    $ sudo usermod -aG vboxusers youruser
    ```
    Проверяем, запущен ли VirtualBox:
    ```
    $ sudo systemctl status vboxdrv
    ```

- **развернуть Vagrant**
    
    На [официальном сайте утилиты](https://www.vagrantup.com/docs/installation/) рекомендую качать инсталлер, но его так же можно установить из репозитория apt:
    ```
    $ sudo apt-get install vagrant
    ```
    При попытке запуска Vagrant иногда можно увидеть следующее сообщение об ошибке:
    ```
    There was an error while executing `VBoxManage`, a CLI used by Vagrant for controlling VirtualBox. The command and stderr is shown below.
    
    Command: ["startvm", <ID of the VM>, "--type", "headless"]

    Stderr: VBoxManage: error: VT-x is being used by another hypervisor (VERR_VMX_IN_VMX_ROOT_MODE).
    VBoxManage: error: VirtualBox can't operate in VMX root mode. Please disable the KVM kernel extension, recompile your kernel and reboot
    (VERR_VMX_IN_VMX_ROOT_MODE)
    VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component ConsoleWrap, interface IConsole
    ```
    Это связано с тем, что в системе на данный момент используется другой гипервизор (например, KVM)
    Проверим:
    ```
    $ lsmod | grep kvm
    kvm_intel             204800  6
    kvm                   593920  1 kvm_intel
    irqbypass              16384  1 kvm
    ```
    Если у вас аналогичное сообщение - добавим этот гипервизор в черный список:
    (в данном случае нас интересует *kvm_intel*, у вас может быть другой)
    ```
    # echo 'blacklist kvm-intel' >> /etc/modprobe.d/blacklist.conf
    ```
    Запускаем Vagrant: 
    ```
    $ vagrant up
    ```
    В случае успешной установки нужно будет некоторое время подождать загрузки и установки образа для виртуальной машины.
    Далее подключаемся к Vagrant:
    ```
    $ vagrant ssh
    ```
    Получаем приглашение вида:
    `vagrant@nomad:~$`
    Проверяем наличие Nomad:
    ```
    vagrant@nomad:~$ nomad
    ```
    Запустим один агент Nomad в режиме разработки. Этот режим используется для быстрого запуска агента, выполняющего роль клиента и сервера, для тестирования конфигураций заданий или взаимодействий прототипов:
    ```
    vagrant@nomad:~$ sudo nomad agent -dev
    ```
    
    
