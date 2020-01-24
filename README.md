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



### 更新 credential

這是一個 public repository, 但在 push 時還是一直會出現帳號密碼錯誤的訊息, 這樣可以解決

    $ git remote set-url origin https://Charles-Hsu:[password]@github.com/Charles-Hsu/gobelieve_vagrant.git
詳細可參考[How to fix Git always asking for user credentials](https://www.freecodecamp.org/news/how-to-fix-git-always-asking-for-user-credentials/)
