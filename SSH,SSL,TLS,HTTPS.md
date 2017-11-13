### References
* [SSL/TLS协议运行机制概述](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)
* [数字签名是什么](http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html)
* [SSH原理与运用](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)


### Understand SSH

1. __什么是SSH__
	* ssh（Secure Shell Protocol）是一种安全的远程登入协议，相比于telnet协议以明文的形式传输数据，用户名，密码，ssh协议将这些内容都进行了加密，可以有效的防止一些网络攻击。通过使用一系列的加密技术，SSH 提供了这样一个机制：建立安全加密连接，互相授权，发送命令和返回输出结果。
	
2. __SSH原理__
	* SSH登陆的两种方式：
		* 口令登入: 原理简单，过程繁琐
		* 公钥登入： 用户需要生成公私钥对，并且将public key事先放在服务器中, 安全性更好，配置之后无需输入密码. 
	* 口令登入过程:
		1. 用户请求登陆
		2. 服务器将自己的[公钥]发给用户，该公钥会被保存在 $HOME/.ssh/known_hosts中. 
		3. 用户用服务器发过来的公钥加密自己的登入密码之后发送回服务器.
		4. 服务器用自己的[私钥]解密之后验证，若通过则同意登陆. 
		5. 由于RSA公钥较长(1024bits), 所以登陆时只比较公钥的md5值(密钥指纹), 因为第一次登入时用户无法确定该公钥是不是服务器的(这种登入方式可能受到中间人攻击), 所以会提示是否信任这个龚玥. 
		6. ![](http://burningcodes.net/wp-content/uploads/2014/12/ssh1.png)

3. __SSH加密__
	1. __对称加密__
		* 一个密钥既可以用来加密消息，也可以用来解密消息。这意味着通信双方都持有相同的密钥，既可以加密又可以解密。这种加密方式又叫做“共享加密”或“密钥加密”。典型的是只有一个密钥或者一对关系紧密的密钥（获得另一个密钥并不重要）。
		* 客户端和服务器一起生成密钥，生成的密钥不会被第三方知道。密钥是通过一个叫做密钥交换算法生成的。交换的结果是服务器和客户端双方获得相同的密钥。这个流程将在后面详细介
		* 称加密密钥的生成是基于session的，并建立了实际传输数据的加密。一旦密钥建立所有数据必须通过共享密钥加密。这是在授权客户端之前完成的。
	2. __非对称加密__
		* 非对称加密中，需要两个密钥：一个叫公钥，另一个叫私钥。公钥可以被放到任何地方。私钥应该保密，不能与其他方共享。私钥不能靠公钥计算获得。公钥和私钥的关系是：公钥加密的数据只能靠私钥解密。这个操作是单向的，也就是说，公钥没有办法解密自己加密的数据
		
### 公钥与私钥
1. 加密方法可以分为两大类：
	* private key cryptography： 通用：DES(Data Encryption Standard)
	* public key cryptography: RSA（Rivest-Sharmir-Adleman）
2. 双钥加密原理：
	1. 公钥和私钥是一一对应关系，有一把公钥就有一把与之对应独一无二的私钥，反之成立
	2. 所有的公私钥对是不同的
	3. 公钥可以解密私钥加密的信息，反之成立
	4. 同时生成公钥和私钥很容易，但是从公钥推算出私钥非常困难或者不可能
	5. 公钥用来加密信息，私钥用来数字签名

### Digital Certificate
1. **Digital Signature**
	* 对信息进行hash处理，生成摘要(digest)
	* 使用私钥对digest进行加密,生成数字签名
	* 签名与信息附在一起
	* 接收方用与发送方的私钥对应的公钥对数字签名进行解密，得到摘要，证实是发送方发送的
	* 接收方对信件本身进行hash产生一个digest,新的digest应该与原digest相同，证明信件未被篡改
2. **Digital Certificate**
	* 客户端的私钥被替换，对应的公钥来自篡改者
	* 要求私钥端去找证书中心(Certificate Authority, CA）为公钥做认证
	* CA用自己的私钥对公钥和相关信息一起加密生成数字证书
	* 发送方在签名同时附上数字证书即可
	* 接收方用CA的公钥解开数字证书，就可以拿到发送方的公钥，再证明数字签名是否是发送方的。
	* 过程
		1. 客户端向服务器发送加密请求
		2. 服务器用自己的私钥加密网页后，连同数字签名一起发送给客户端
		3. 客户端(浏览器)的证书管理器，有“受信任的根证书颁布机构”列表，客户端根据这个列表查看解开数字证书的公钥(来自CA）是否在列表中
		4. 如果可靠，客户端用证书中的公钥加密解开服务器的公钥，再用该公钥对要发送的信息加密，与服务器交换加密信息。
	
### Https
1. 需求:解决http(不使用SSL/TLS)的风险
	* eavesdropping: 通信内容不加密，第三方监控
	* tampering: 第三方修改通信内容
	* pretending: 第三方冒充他人进行通信
2. SSL/TLS
	* 所有信息加密传播，无法被窃听
	* 具有校验机制，一旦被篡改，通信双方立刻被发现
	* 配备身份证书，防止身份被冒充
3. 如何保证公钥不被篡改： 公钥放在数字证书中
4. 公钥加密计算量大，如何减少耗时
	* 每一次对话(session)，客户端和服务器生成一个对话秘钥(session key),用来加密信息，对称加密，速度非常快
	* 服务器公钥只用来加密session key本身
5. SSL/TLS协议过程
	1. 客户端向服务器索要并验证公钥
	2. 双方协商生成session key
	3. 使用session key进行加密通信
6 SSL/TLS 四次握手
	1. 客户端发送请求
		* 协议版本，例如TLS 1.0
		* 客户端生成的随机数，稍后用于生成对话秘钥
		* 支持的加密方法，例如RSA公钥加密
		* 支持的压缩方法	
	2. 服务器响应
		* 确认的协议版本，如果与客户端不同，关闭加密通信
		* 服务器生成的随机数，稍后用于对话秘钥
		* 使用的加密方法
		* 服务器证书
	3. 客户端回应
		* 首先验证服务器证书，取出服务器公钥
		* 发送随机数，用服务器公钥加密，防止被窃听
		* 编码改变通知，表示随后的信息都用双方商定的加密方法和秘钥传送
		* 客户端握手结束通知
	4. 服务器最后回应
		* 受到客户端的第三个随机数pre-master key之后生成本次对话用的会话秘钥
		* 发哦送你个编码改变通知
		* 发送服务器握手结束通知
	5. 握手阶段结束，双方进入加密通信，完全是普通的http协议，只不过用会话秘钥加密内容

