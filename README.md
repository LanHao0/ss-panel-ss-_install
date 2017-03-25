# shawdowsocks前后端_install_BY：兰兰想<br>
#转载请保留此信息<br>
#教程仅适用于 20170325版V3魔改，如果找不到的话我的Fork了一份…<br>
#我的环境 : 搬瓦工 512RAM Ubuntu 14 x64<br>
#教程参考网站：https://imeiji.github.io/2017/01/09/%E6%90%AD%E5%BB%BA%20sspanel%20v3%20%E9%AD%94%E6%94%B9%E7%89%88%E8%AE%B0%E5%BD%95/<br>
#其他参考网站：https://iforday.com/160.html<br>
#https://stackoverflow.com/questions/9236953/bash-chattr-command-not-found/35169660#35169660?newreg=31d442f0eacd4007bf6a5542bc58ad20<br>
#用最简单的方式进行 安装与运行<br>

1.安装screen（后期也要靠它保持后端的运行）<br>
apt-get install screen<br>
然后输入命令screen直接回车两下<br>

2.安装LNMP一键包<br>
虽然内存只有512，但是！更推荐使用1.3一键包，因为我用1.2的时候出错了。<br>
wget http://soft.vpser.net/lnmp/lnmp1.3-full.tar.gz && tar xvzf lnmp1.3-full.tar.gz<br>
cd lnmp1.3-full && ./install.sh<br>
输入MySQL密码（记下来，别忘了）, 用MYSQL5.5，php7.0<br>
出现enjoy it绿字后，继续下一步<br>

3.添加网站<br>
lnmp vhost add<br>
输入域名, 然后其它选 no<br>

4.修改 nginx 配置<br>
winscp登陆，修改 这个目录/usr/local/nginx/conf/vhost/你的域名.conf<br>
sever段增加<br>
location / <br>
{<br>
	try_files $uri $uri/ /index.php$is_args$args;		     <br>           
}<br>
<br>
root那一行后面加public，别忘了分号；<br>
root /home/wwwroot/你的域名/public;<br>

/usr/local/nginx/conf/<br>
这个目录中的nginx.conf，server段，中的server name推荐改为你的域名 + www你的域名，如下：<br>
        server_name www.你的域名 你的域名;<br>


5.网站前端ss-panel 安装<br>
cd /home/wwwroot/你的域名<br>
apt-get install git -y<br>
git clone -b master https://github.com/esdeathlove/ss-panel-v3-mod.git tmp && mv tmp/.git . && rm -rf tmp && git reset --hard<br>
chown -R root:root *<br>
chmod -R 755 *<br>
chown -R www:www storage<br>

6.安装依赖<br>
apt-get install e2fsprogs<br>
（为了使下面的chattr 命令可以运行）<br>

7.继续安装前端，确保你的目录依旧在/home/wwwroot/你的域名<br>
chattr -i .user.ini<br>
mv .user.ini public<br>
cd public<br>
chattr +i .user.ini<br>
service nginx restart<br>

8.配置网站前端数据库<br>
http://你的域名/phpmyadmin<br>
如果域名暂时未生效，可以先用ip，更推荐将自己电脑DNS改为google的8.8.8.8和8.8.4.4，既可以防污染又可以更快地访问域名<br>
用户 : root<br>
密码 :安装 lnmp 时设置的，我提醒过你别忘了！<br>
登录后，上面 用户 -> 新建 -> 添加用户  
Username 使用文本域 , 填写你的用户名  
Host 任意主机 %  
密码 使用文本域 填写密码  
用户数据库 :  
勾选 <b>创建与用户同名的数据库并授予所有权限  </b>
全局权限 :  全选  
按执行 选择刚刚新建的数据库  导入网站目录下的 glzjin_all.sql  
没写完以后写
