## 数据库的那些事儿
### 提纲
1. 什么是数据库？
	1. 数据库从何而来？
	2. 计算机的存储层级
	3. 数据库的种类
	4. RDBMS/SQL
	5. 什么是关系？
2. 关系型数据库
	1. 基本操作：CRUD
	2. 事务
	2. 关系型数据库的性质:ACID
	3. 常见关系型数据库
	4. 
	
	
	
	
	

### 什么是数据库？
1. **从古至今，数据存在哪**？
	* 当我们没有接触数据库时，如果我们想把某些数据保留在电脑上，我们经常会打开windows自带的文本编辑器或者word等软件然后把数据输进去。
	* 当数据量大幅增长之后，在word里搜索数据变得如此痛苦：数据库软件应运而生。
	* 内存中的内容并不具备持久性，当一个进程(process)结束后，它所占据的内存就要被释放。磁盘/文件系统(某种程度上这是一个事物的两个描述)具备持久性。
2. **计算机的存储层级**
	* 寄存器(register)：CPU中，bit级大小，速度极快, 不具备持久性(persistency)
	* 缓存(cache): CPU中， KB级大小，速度极快， 不具备持久性
	* 内存(memory): 主板上, G级大小， 速度快， 不具备持久性
	* 磁盘(disk/file system): 主板外， TG, PG级大小， 速度慢， 具备持久性
	* 规律：距离CPU越近，大小越小，速度越快
3. **数据库的种类**
 	* 关系型数据库：出现较早，发展完备，使用最广泛
	* 非关系型数据库： 不使用“关系”的数据库总称，包含列数据库，文档数据库，图数据库等等
4. **RDBMS与SQL**
	1. RDBMS也就是Relational Database Management System:关系型数据库管理系统。我们说的MySQL数据库等就是RDBMS. RDBMS是一整套软件，为人们提供各式数据存储，查询服务
	2. SQL是Structural Query Language: 结构化查询语言。它定义了一套规则以方便使用者对RDBMS进行查询等服务。
	3. 一般来说，SQL数据库，RDBMS我们可以混淆这个概念，都指的是“关系型”数据库。
5. **关系**
	1. 让我们慢慢来体会关系型数据库中的关系到底指什么.
	
### 关系型数据库
1. 基本操作：CRUD
	* 泛泛来说，任何一个系统一般都会考虑两种操作：读与写（read/write). 前者是从系统中获取信息，后者是向系统中写入信息。我们具体把写操作扩展细化一下就形成了CRUD操作，也就是增查改删。
	* Create: 属于”写“操作，增加新的数据
	* Read: 属于”读“操作， 读取数据
	* Update: 属于”写“操作，更新数据
	* Delete: 属于”写“操作，删除数据
	* 注意CRUD操作并不只是针对数据库的操作，事实上大多数计算机系统都可以提供CRUD操作。
2. 事务(Transaction)
	* 事务是关系型数据库中最重要的一个概念！
	* 如何开启一个事务？
	
	
