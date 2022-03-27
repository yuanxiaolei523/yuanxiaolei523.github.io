# nodejs learn

## NodeJS操作数据库
### 1. 连接
这里我附上我开发时的[参考连接]('https://www.runoob.com/nodejs/nodejs-mysql.html')，然后下面将写一下我自己整理的一些东西

1. 首先在项目中执行 `npm i mysql ` 去安装mysql

2. 新建一个mysql.js文件

3. 在文件引入mysql文件

   ```js
   var mysql = require('mysql');
   ```


4. 引入后与mysql数据库创建连接，并建立连接

   ```js
   var connection = mysql.createConnection({
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
    这里需要把`localhost`和`password`替换一下

2. ```
   flush privileges;
   ```

