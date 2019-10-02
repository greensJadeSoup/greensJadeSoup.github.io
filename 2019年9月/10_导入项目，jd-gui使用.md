## 版本不兼容
maven错误提示Failed to read artifact descriptor for org.apache.hadoop:hadoop-common:jar:2.4.0
## 解决方法
+ 打开项目后，在Intellij 右侧有个Maven projects，点开后，有个Lifecycle，再点开，可以看到clean , validate, compile, ….，右击clean，选中Run ‘project[clean]’，这里的project是我们的项目实际的名字。
+ 在同样的地方（Lifecycle)里找到install, 选中Run ‘project[install]’，这里的project同样是我们的具体项目的名字，这个过程比较久，如果有遇到哪个jar包不能下载的情况，可以手动将其放到本地的m2目录下，
+ 最后，找到Maven projects，点击“Reimport All Maven Projects”，这个时候，错误”Failed to ….”消失，需要的依赖开始下载。
## jd-gui
### 在jdk1.8使用
jd-gui适用于jdk1.7，1.8想使用则  
进入路径 java -jar jd-gui.exe
### 使用
