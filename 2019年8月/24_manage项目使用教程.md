一.项目启动前需安装：
jdk1.8.0_191
maven3.6.1
intellij IDEA 2019
mysql8.0.17，安装教程：https://www.cnblogs.com/laumians-notes/p/9069498.html
二.主要目录
manage
	-src
		-main 主程序
			- java java程序
				-com.wolf.material
					-controller 业务模块控制层
						-SoftwareController 软件组成员模块控制层
					-mapper 业务逻辑处理层
						-SoftwareInfoMapper 软件组成员sql语句
					-pojo  实体类层
						-SoftwareInfo 软件组成员实体类
					-service 应用逻辑接口
						-Impl 实体类
							-SoftwareInfoServiceImpl 软件组成员逻辑实体类
						-SoftwareInfoService 软件组成员逻辑接口
			- resources 资源存放
				-application.yml 数据库，连接池等配置（后续扩展）
				-json.html 测试跨域json交互的网页
				-js 存放js文件
					-jquery-3.4.1.min.js
		-test 测试程序
二.项目简单使用：
1.IDEA配置jdk，maven等
2.在mysql新建数据库，名为wolf_material,新建表softwareInfo
表创建sql语句如下：
DROP TABLE
IF
	EXISTS `softwareInfo`;
CREATE TABLE `softwareInfo` (
`sw_id` INT ( 50 ) NOT NULL AUTO_INCREMENT COMMENT '成员ID',
`sw_name` VARCHAR ( 100 ) NOT NULL COMMENT '成员姓名',
`sw_sex` VARCHAR ( 30 ) NOT NULL COMMENT '性别',
`sw_birthday` DATE NOT NULL COMMENT '出生日期',
`sw_grade` VARCHAR ( 30 ) NOT NULL COMMENT '年级',
`sw_major` VARCHAR ( 100 ) NOT NULL COMMENT '专业',
PRIMARY KEY ( `sw_id` ) 
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4;
INSERT INTO `softwareInfo`
VALUES
	( NULL, '黄彦钊', '男', '1999-10-30', '2017', '自动化' ),
	( NULL, '李四', '男', '1999-10-30', '2017', '电信' ),
	( NULL, '张三', '男', '1999-10-30', '2017', '通信' );
3.在IDEA内打开项目，打开com.wolf.material目录下的ManageApplication文件，直接运行，端口默认为8080
4.启动成功后，用非ie浏览器打开网址http://localhost:63342/manage/json.html，或者直接在IDEA内选择resources目录下的json.html用浏览器打开，出现网页
5.按f12调出控制台，查看返回四个按钮的功能显示数据，详细说明在代码中，目前已经实现了互相传递json跨域，并且能查询mysql数据库里面的数据，整合成功。
三.项目复杂使用
1.该项目配制了druid连接池，可以通过localhost:8080/druid访问，详细功能自行探索。
账号:admin,密码:123456
2.后续会添加redis缓存，transaction事务管理，springSecurity权限控制等

