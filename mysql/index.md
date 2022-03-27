# mysql的学习日志

## 安装mysql
1. 到社区下载[mysql](https://dev.mysql.com/downloads/mysql/) 安装包

2. 根据自己的系统选择合适的安装包

   ![image-20220327002423786](/Users/yuanxiaolei/Library/Application Support/typora-user-images/image-20220327002423786.png)

   我这里选择的是界面化安装的x86 64位的安装包

3. 等待下载完成后，开始图形化界面，一步步的安装即可

## 二、启动Mysql

前面我们已经下载完了mysql，那么我们应该去哪里找到它呢，又应该去哪启动它呢，此时肯定有一些小可爱会说到，我安装了mysql，肯定去application里面找啊， 如果这个时候你去mysql里面找，你会发现根本找不到，事实上，他出现在系统偏好设置里面了

![image-20220327002805282](/Users/yuanxiaolei/Library/Application Support/typora-user-images/image-20220327002805282.png)

在最下面，我们可以看到MySQL的影子，此时我们点击进去，启动mysql

![image-20220327003032588](/Users/yuanxiaolei/Library/Application Support/typora-user-images/image-20220327003032588.png)

只要是图片上这个样子，就表明你的mysql已经启动。

## 三、配置环境变量

在我们启动了`MySQL`之后此时有些小可爱会很兴奋的迫不及待的去命令行里面操作了。但是真当我们到命令行输入`mysql`的时候，会提示`commod not found`，这是因为我们需要对mysql配置一个环境变量，将mysql指向我们安装的位置即可

![image-20220327003450076](/Users/yuanxiaolei/Library/Application Support/typora-user-images/image-20220327003450076.png)

1. 执行`open ~/.zshrc`,打开我们zsh的配置文件
2. 在配置文件内部新增`PATH=$PATH:/usr/local/mysql/bin`， 然后保存退出
3. 退出之后`source ~/.zshrc`，使其立即生效.
4. 此时我们再次在命令行输入`mysql -uroot -p`，然后按照提示输入你的密码即可正常访问

## 四、安装后的配置

1. 在安装完成之后，mysql是没有设置任何密码的，此时会存在安全隐患，所以我们需要设置一下mysql的密码，在命令行中输入`mysqladmin -u root password "new_password";`即可，记得将new_password改成自己要设置的密码
2. 改完之后，我们可以通过下面的命令来访问登录mysql`mysql -u root -p`，回车后输入你的mysql密码即可



## 五、在进入登入mysql之后如何退出呢？

如果想要退出mysq，那么我们仅需要在命令行中输入`exit`回车即可

![image-20220327003954413](/Users/yuanxiaolei/Library/Application Support/typora-user-images/image-20220327003954413.png)

## 六、图形化界面操作mysql

如果觉得命令行操作`Mysql`比较繁琐，或者你记不住那么多的sql语句，那么使用[Navicat]('https://www.navicat.com.cn/')进行`图形化`界面操作Mysql就显得尤为重要了，当然Navicat正版是需要花钱的，如果想白嫖的话，可以使用[破解版]('https://xclient.info/search/s/navicat/')

> 我在说明一下：大家一定要支持正版！！！尊重版权



## 六、NodeJS如何连接mysql

这里我附上我开发时的[参考连接]('https://www.runoob.com/nodejs/nodejs-mysql.html')，然后下面将写一下我自己整理的一些东西

1. 首先在项目中执行 `npm i mysql ` 去安装mysql

2. 新建一个mysql.js文件

3. 在文件引入mysql文件

   ```js
   const mysql = require('mysql');
   ```

   

4. 引入后与mysql数据库创建连接，并建立连接

   ```js
   const connection = mysql.createConnection({
       host     : 'localhost', // host
       user     : 'root', // user
       password : 'xxxxx', // mysql的登录密码
       database : 'first_test' // 数据库表
   });
   
   connection.connect();
   ```

   

5. 查询blog表中的数据

   ```js
   const StringDecoder = require('string_decoder').StringDecoder;
   const decoder = new StringDecoder('utf8');
   
   connection.query('SELECT * from blog', function (error, results, fields) {
       if (error) {
           console.log(error);
           throw error;
       }
       Object.keys(results[0]).map((key) => { // 这里是因为输出的内容是Buffer类型的，我们需要将其转换为字符串
           console.log(decoder.write(results[0][key]));
       })
       console.log('The solution is: ', results[0]);
   });
   ```

### 连接过程中如果有报错怎么办

#### 1. Error: consider upgrading MySQL client

![image-20220327195830042](/Users/yuanxiaolei/Library/Application Support/typora-user-images/image-20220327195830042.png)

如果有这样的报错，那么可以这样解决

1. ```
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
   ```

​	这里需要把`localhost`和`password`替换一下

2. ```
   flush privileges;
   ```

