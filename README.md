# gobelieve vagrant
1. vagrant box add ubuntu/xenial64

2. cd $dir(Vagrantfile) && vagrant up

3. vagrant ssh

#### 安裝 redis server
詳細參考這裡 [How to Install Redis on Ubuntu 18.04 & 16.04 LTS](https://tecadmin.net/install-redis-ubuntu/)

    $ sudo apt-get install redis-server

#### 執行 redis server

    sudo redis-server /etc/redis.conf

#### 安裝 git
    
    $ sudo apt-get install git
    
#### 把 gobelieve_vagrant cloen 回來

    $ git clone https://github.com/Charles-Hsu/gobelieve_vagrant.git
    
#### 複製所有的東西到 /vagrant

    $ cd gobelieve_vagrant
    $ cp -rf *.* /vagrant/
    
#### 執行 setup.sh

    $ cd /vagrant
    $ sudo ./setup.sh

5. cd /data/wwwroot/im_bin && sudo ./run.sh start

###### 確認執行是否正常
    $ sudo netstat -tulpn | grep LISTEN
    
![Alt text](https://github.com/Charles-Hsu/gobelieve_vagrant/blob/master/vagrant-netstat.png)

6. 获取token

   python /data/wwwroot/cli/gobelieve_auth.py $uid $name

7. 群组操作

##### 修訂 /data/wwwroot/cli/rpc.py 程式碼

    $ sudo vi /data/wwwroot/cli/rpc.py
    try:
        from urllib.parse import urlparse
    except ImportError:
        from urlparse import urlparse
    #from urllib.parse import urlencode
    
##### 建立群組

 python /data/wwwroot/cli/gobelieve_group.py create $master $group_name $is_super $m1 $m2 $m3...
 
    $ python /data/wwwroot/cli/gobelieve_group.py create 1 2 3 4 5 6
    ('new group id:', 1)
    $ python /data/wwwroot/cli/gobelieve_group.py create 1 2 3 4 5 6
    ('new group id:', 2)
    
    $ mysql -uroot -pGoBelieve123456
    
![](https://github.com/Charles-Hsu/gobelieve_vagrant/blob/master/vagrant-select.png)
    

###### python /data/wwwroot/cli/gobelieve_group.py add_member $gid $m1 $m2 $m3...
    
    $ python /data/wwwroot/cli/gobelieve_group.py add_member $gid $m1 $m2 $m3...

   python /data/wwwroot/cli/gobelieve_group.py remove_member $gid $m1 $m2 $m3...

   python /data/wwwroot/cli/gobelieve_group.py delete $gid

   python /data/wwwroot/cli/gobelieve_group.py upgrade $gid

8. 测试
   python /data/wwwroot/cli/gobelieve_client.py

#### 設定 Vagrant 可以由外界 ping 進來

參考這篇文章 [Vagrant: can not ping guest machine from the host](https://stackoverflow.com/a/25457532/3309645)

- 把 VirtualBox 裡頭的 VM 網路設定為 Bridge ---> Doesn't work, 會有 error
- 編輯 Vagrantfile

      config.vm.network "public_network"
      
- 重新啟動 Vagrant

      vagrant halt
      vagrant up

![](https://github.com/Charles-Hsu/gobelieve_vagrant/blob/master/vagrant-bridge-up.png)

- 進入 vagrant

      vagrant ssh
      ifconfig
      
 這樣子就可以看到被 assign 的 IP
 
 ![](https://github.com/Charles-Hsu/gobelieve_vagrant/blob/master/vagrant-ifconfig.png)

### 讓 MySql 可以由外面 Host 連進來

    $ mysql -u root -p
    mysql> use mysql
    mysql> GRANT ALL ON *.* TO root@'192.168.1.101' IDENTIFIED BY 'GoBelieve123456';
    mysql> UPDATE user SET Host='192.168.1.101' WHERE Host='*';
    mysql> SELECT * FROM user WHERE Host='192.168.1.101';
    mysql> FLUSH PRIVILEGES;
    
修改 my.cnf 檔案

    $ sudo vi /etc/mysql/my.cnf
    #bind-address           = 127.0.0.1
    bind-address            = 0.0.0.0
    
重新啟動 MySql

    $ sudo /etc/init.d/mysql restart
    
到 Host 執行下列指令

    $ mysql -uroot -p -h192.168.1.120
    
![](https://github.com/Charles-Hsu/gobelieve_vagrant/blob/master/vagrant-mysql-from-host.png)

執行了 cli/ 下面的 gobelieve_client.py, 但連不進來

#### 如何確認 VM 的 port 的狀態

執行這個指令可以確認 port 13333 是有被 listen 的

    $ sudo tcpdump -i any port 13333
    
![](https://github.com/Charles-Hsu/gobelieve_vagrant/blob/master/vagrant-port-13333.png)
![](https://github.com/Charles-Hsu/gobelieve_vagrant/blob/master/vagrant-host-python.png)

### 更新 credential

這是一個 public repository, 但在 push 時還是一直會出現帳號密碼錯誤的訊息, 這樣可以解決

    $ git remote set-url origin https://Charles-Hsu:[password]@github.com/Charles-Hsu/gobelieve_vagrant.git
詳細可參考[How to fix Git always asking for user credentials](https://www.freecodecamp.org/news/how-to-fix-git-always-asking-for-user-credentials/)
