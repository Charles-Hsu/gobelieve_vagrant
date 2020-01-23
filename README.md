# gobelieve vagrant
1. vagrant box add ubuntu/xenial64

2. cd $dir(Vagrantfile) && vagrant up

3. vagrant ssh

#### 安裝 redis server
詳細參考這裡 [How to Install Redis on Ubuntu 18.04 & 16.04 LTS](https://tecadmin.net/install-redis-ubuntu/)

    $ sudo apt-get install redis-server

4. sudo redis-server /etc/redis.conf

#### 安裝 git
    $ sudo apt-get install git

5. cd /data/wwwroot/im_bin && sudo ./run.sh start

6. 获取token

   python /data/wwwroot/cli/gobelieve_auth.py $uid $name

7. 群组操作

   python /data/wwwroot/cli/gobelieve_group.py create $master $group_name $is_super $m1 $m2 $m3...

   python /data/wwwroot/cli/gobelieve_group.py add_member $gid $m1 $m2 $m3...

   python /data/wwwroot/cli/gobelieve_group.py remove_member $gid $m1 $m2 $m3...

   python /data/wwwroot/cli/gobelieve_group.py delete $gid

   python /data/wwwroot/cli/gobelieve_group.py upgrade $gid

8. 测试
   python /data/wwwroot/cli/gobelieve_client.py


### 更新 credential

這是一個 public repository, 但在 push 時還是一直會出現帳號密碼錯誤的訊息, 這樣可以解決
    $ git remote set-url origin https://Charles-Hsu:Github123@github.com/Charles-Hsu/gobelieve_vagrant.git
