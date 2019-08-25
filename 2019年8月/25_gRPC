# 证书
进入网站https://freessl.org/，输入域名生成证书
得到pem，keyt文件
+ MAC下直接命令生成.p12文件,过程中需要输入密码，记好！
openssl pkcs12 -export -inkey private.key -in full_chain.pem -name tomcat -out tomcat.p12
+ 通过keytool生成.jks文件
keytool -importkeystore -srckeystore C:\tomcat.p12 -srcstoretype pkcs12 -destkeystore C:\tomcat.jks
+ tomcat中配置
```java
<Connector ... keystoreFile="C:\xxx.jks" keystorePass="xxx">
```
# gRPC
gRPC 基于 HTTP/2 标准设计，带来诸如双向流、流控、头部压缩、单 TCP 连接上的多复用请求等特。这些特性使得其在移动设备上表现更好，更省电和节省空间占用。一开始由 google 开发，是一款语言中立、平台中立、开源的远程过程调用(RPC)系统。
在 gRPC 里客户端应用可以像调用本地对象一样直接调用另一台不同的机器上服务端应用的方法，使得您能够更容易地创建分布式应用和服务。与许多 RPC 系统类似，gRPC 也是基于以下理念：定义一个服务，指定其能够被远程调用的方法（包含参数和返回类型）。在服务端实现这个接口，并运行一个 gRPC 服务器来处理客户端调用。在客户端拥有一个存根能够像服务端一样的方法。
## protocol buffers3（protobuf3）
gRPC 默认使用 protocol buffers，这是 Google 开源的一套成熟的结构数据序列化机制（当然也可以使用其他数据格式如 JSON）,用于结构化数据串行化的灵活、高效、自动的方法，例如XML，不过它比xml更小、更快、也更简单。正如你将在下方例子里所看到的，你用 proto files 创建 gRPC 服务，用 protocol buffers 消息类型来定义方法参数和返回类型。
### 下载与配置
下载 protocol buffer 的编译器和相应类库。下载地址为：http://code.google.com/p/protobuf/downloads/list 

### 工作原理
在.proto文件定义消息，message是.proto文件最小的逻辑单元，由一系列name-value键值对构成。
+ 文件的第一行指定了你正在使用proto3语法：如果你没有指定这个，编译器会使用proto2。这个指定语法行必须是文件的非空非注释的第一个行。
+ 每个字段都有一个名字和一种类型，等号右边的值不是字段默认值，而是数字标签，是字段身份的标识符，类似于数据库中的主键；
```proto
syntax = "proto3";

message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phone = 4;
}
```
message消息包含一个或多个编号唯一的字段，每个字段由字段限制,字段类型,字段名和编号四部分组成，字段限制分为：optional(可选的)、required(必须的-uf编译器生成C++对应的.h和.cc文件，源文件提供了message消息的序列化和反序列化等方法：
```proto
# 序列化数据
Person person;
person.set_name("John Doe");
person.set_id(1234);
person.set_email("jdoe@example.com");
fstream output("myfile", ios::out  | ios::binary);
person.SerializeToOstream(&output);

# 反序列化数据
fstream input("myfile", ios::in  | ios::binary);
Person person;
person.ParseFromIstream(&input);cout <<  "Name: "  << person.name()  << endl;cout <<  "E-mail: "  << person.email()  << endl;
```
