easyway-dubbo
=============
由于开源站点因为安全问题被下掉，如果编译时出现找不到opensesame依赖情况的，请先手动下载https://github.com/alibaba/opensesame
本地install，这几天我们会把opensesame发到mavan中央仓库。

临时文档地址：http://alibaba.github.io/dubbo-doc-static/Developer+Guide-zh.htm

================================================================

Dubbo is a distributed service framework enpowers applications with service import/export capability with high performance RPC.

It's composed of three kernel parts:

Remoting: a network communication framework provides sync-over-async and request-response messaging.

Clustering: a remote procedure call abstraction with load-balancing/failover/clustering capabilities.

Registry: a service directory framework for service registration and service event publish/subscription

For more, please refer to:

    http://code.alibabatech.com/wiki/display/dubbo

================================================================
Quick Start
================================================================

Export remote service:

    <bean id="barService" class="com.foo.BarServiceImpl" />
	
    <dubbo:service interface="com.foo.BarService" ref="barService" />

Refer remote service:

    <dubbo:reference id="barService" interface="com.foo.BarService" />
	
    <bean id="barAction" class="com.foo.BarAction">
        <property name="barService" ref="barService" />
    </bean>

================================================================
Source Building
================================================================

0. Install the git and maven command line:

    yum install git
    or: apt-get install git

    cd ~
    wget http://www.apache.org/dist//maven/binaries/apache-maven-2.2.1-bin.tar.gz
    tar zxvf apache-maven-2.2.1-bin.tar.gz
    vi .bash_profile
       - edit: export PATH=$PATH:~/apache-maven-2.2.1/bin
    source .bash_profile

1. Checkout the dubbo source code:

    cd ~
    git clone https://github.com/alibaba/dubbo.git dubbo

    git checkout -b dubbo-2.4.0
    git checkout master

2. Import the dubbo source code to eclipse project:

    cd ~/dubbo
    mvn eclipse:eclipse
    Eclipse -> Menu -> File -> Import -> Exsiting Projects to Workspace -> Browse -> Finish

    Context Menu -> Run As -> Java Application:
    dubbo-demo-provider/src/test/java/com.alibaba.dubbo.demo.provider.DemoProvider
    dubbo-demo-consumer/src/test/java/com.alibaba.dubbo.demo.consumer.DemoConsumer
    dubbo-monitor-simple/src/test/java/com.alibaba.dubbo.monitor.simple.SimpleMonitor
    dubbo-registry-simple/src/test/java/com.alibaba.dubbo.registry.simple.SimpleRegistry

    Edit Config:
    dubbo-demo-provider/src/test/resources/dubbo.properties
    dubbo-demo-consumer/src/test/resources/dubbo.properties
    dubbo-monitor-simple/src/test/resources/dubbo.properties
    dubbo-registry-simple/src/test/resources/dubbo.properties

3. Build the dubbo binary package:

    cd ~/dubbo
    mvn clean install -Dmaven.test.skip
    cd dubbo/target
    ls

4. Install the demo provider:

    cd ~/dubbo/dubbo-demo-provider/target
    tar zxvf dubbo-demo-provider-2.4.0-assembly.tar.gz
    cd dubbo-demo-provider-2.4.0/bin
    ./start.sh

5. Install the demo consumer:

    cd ~/dubbo/dubbo-demo-consumer/target
    tar zxvf dubbo-demo-consumer-2.4.0-assembly.tar.gz
    cd dubbo-demo-consumer-2.4.0/bin
    ./start.sh
    cd ../logs
    tail -f stdout.log

6. Install the simple monitor:

    cd ~/dubbo/dubbo-simple-monitor/target
    tar zxvf dubbo-simple-monitor-2.4.0-assembly.tar.gz
    cd dubbo-simple-monitor-2.4.0/bin
    ./start.sh
    http://127.0.0.1:8080

7. Install the simple registry:

    cd ~/dubbo/dubbo-simple-registry/target
    tar zxvf dubbo-simple-registry-2.4.0-assembly.tar.gz
    cd dubbo-simple-registry-2.4.0/bin
    ./start.sh

    cd ~/dubbo/dubbo-demo-provider/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=dubbo://127.0.0.1:9090
    cd ../bin
    ./restart.sh

    cd ~/dubbo/dubbo-demo-consumer/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=dubbo://127.0.0.1:9090
    cd ../bin
    ./restart.sh

    cd ~/dubbo/dubbo-simple-monitor/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=dubbo://127.0.0.1:9090
    cd ../bin
    ./restart.sh

8. Install the zookeeper registry:

    cd ~
    wget http://www.apache.org/dist//zookeeper/zookeeper-3.3.3/zookeeper-3.3.3.tar.gz
    tar zxvf zookeeper-3.3.3.tar.gz
    cd zookeeper-3.3.3/conf
    cp zoo_sample.cfg zoo.cfg
    vi zoo.cfg
       - edit: dataDir=/home/xxx/data
    cd ../bin
    ./zkServer.sh start

    cd ~/dubbo/dubbo-demo-provider/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=zookeeper://127.0.0.1:2181
    cd ../bin
    ./restart.sh

    cd ~/dubbo/dubbo-demo-consumer/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=zookeeper://127.0.0.1:2181
    cd ../bin
    ./restart.sh

    cd ~/dubbo/dubbo-simple-monitor/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=zookeeper://127.0.0.1:2181
    cd ../bin
    ./restart.sh

9. Install the redis registry:

    cd ~
    wget http://redis.googlecode.com/files/redis-2.4.8.tar.gz
    tar xzf redis-2.4.8.tar.gz
    cd redis-2.4.8
    make
    nohup ./src/redis-server redis.conf &

    cd ~/dubbo/dubbo-demo-provider/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=redis://127.0.0.1:6379
    cd ../bin
    ./restart.sh

    cd ~/dubbo/dubbo-demo-consumer/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=redis://127.0.0.1:6379
    cd ../bin
    ./restart.sh

    cd ~/dubbo/dubbo-simple-monitor/conf
    vi dubbo.properties
       - edit: dubbo.registry.adddress=redis://127.0.0.1:6379
    cd ../bin
    ./restart.sh

10. Install the admin console:

    cd ~/dubbo/dubbo-admin
    mvn jetty:run -Ddubbo.registry.address=zookeeper://127.0.0.1:2181
    http://root:root@127.0.0.1:8080

Git常用操作命令收集：
1) 远程仓库相关命令
检出仓库：$ git clone git://github.com/jquery/jquery.git
查看远程仓库：$ git remote -v
添加远程仓库：$ git remote add [name] [url]
删除远程仓库：$ git remote rm [name]
修改远程仓库：$ git remote set-url --push[name][newUrl]
拉取远程仓库：$ git pull [remoteName] [localBranchName]
推送远程仓库：$ git push [remoteName] [localBranchName]

2）分支(branch)操作相关命令
查看本地分支：$ git branch
查看远程分支：$ git branch -r
创建本地分支：$ git branch [name] ----注意新分支创建后不会自动切换为当前分支
切换分支：$ git checkout [name]
创建新分支并立即切换到新分支：$ git checkout -b [name]
删除分支：$ git branch -d [name] ---- -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项
合并分支：$ git merge [name] ----将名称为[name]的分支与当前分支合并
创建远程分支(本地分支push到远程)：$ git push origin [name]
删除远程分支：$ git push origin :heads/[name]
我从master分支创建了一个issue5560分支，做了一些修改后，使用git push origin master提交，但是显示的结果却是'Everything up-to-date'，发生问题的原因是git push origin master 在没有track远程分支的本地分支中默认提交的master分支，因为master分支默认指向了origin master 分支，这里要使用git push origin issue5560：master 就可以把issue5560推送到远程的master分支了。

    如果想把本地的某个分支test提交到远程仓库，并作为远程仓库的master分支，或者作为另外一个名叫test的分支，那么可以这么做。

$ git push origin test:master         // 提交本地test分支作为远程的master分支
$ git push origin test:test              // 提交本地test分支作为远程的test分支

如果想删除远程的分支呢？类似于上面，如果:左边的分支为空，那么将删除:右边的远程的分支。

$ git push origin :test              // 刚提交到远程的test将被删除，但是本地还会保存的，不用担心
3）版本(tag)操作相关命令
查看版本：$ git tag
创建版本：$ git tag [name]
删除版本：$ git tag -d [name]
查看远程版本：$ git tag -r
创建远程版本(本地版本push到远程)：$ git push origin [name]
删除远程版本：$ git push origin :refs/tags/[name]

4) 子模块(submodule)相关操作命令
添加子模块：$ git submodule add [url] [path]
如：$ git submodule add git://github.com/soberh/ui-libs.git src/main/webapp/ui-libs
初始化子模块：$ git submodule init ----只在首次检出仓库时运行一次就行
更新子模块：$ git submodule update ----每次更新或切换分支后都需要运行一下
删除子模块：（分4步走哦）
1)$ git rm --cached [path]
2) 编辑“.gitmodules”文件，将子模块的相关配置节点删除掉
3) 编辑“.git/config”文件，将子模块的相关配置节点删除掉
4) 手动删除子模块残留的目录

5）忽略一些文件、文件夹不提交
在仓库根目录下创建名称为“.gitignore”的文件，写入不需要的文件夹名或文件，每个元素占一行即可，如
target
bin
*.db
分类: 其他