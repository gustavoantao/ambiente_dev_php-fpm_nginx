# Creating the environment

```
mkdir ~/development/
cd ~/development/
git clone https://github.com/gustavoantao/ambiente_dev_php-fpm_nginx.git containers
cd containers/composer
sudo vi /etc/hosts
```
Add to /etc/hosts the following line:
```
172.16.222.2 app1.dev.local app2.dev.local
```
Run:
```
docker-compose up -d
```
This will setup the 2 example apps. To run more apps just put it in `wwwdata` folder and create a new vhost in `config`.

Try it:
[App1](http://app1.dev.local)
[App2](http://app2.dev.local)
