本文主要整理了Splunk中各项技术的使用，以下代码可以反复运行。

## 0. Before

1. Centos下安装[Splunk Enterprise](https://www.splunk.com/)。

   首先在官网下载Splunk Enterprise，然后修改下面语句中前两个参数，然后执行。

   ~~~shell
   install_file=~/Downloads/splunk-8.0.1-6db836e2fb9e-Linux-x86_64.tgz  # 安装文件所在路径
   install_path=$HOME/eipi10/aa/              # 指定splunk安装所在路径
   
   tar xvzf $install_file -C $install_path
   
   if [ `grep -c 'SPLUNK_HOME' ~/.bashrc` -eq 0 ];then  
     echo                                               >> ~/.bashrc
     echo export SPLUNK_HOME=$install_path/splunk       >> ~/.bashrc
     echo export PATH=\"\$PATH:\$SPLUNK_HOME/bin\"      >> ~/.bashrc
   fi
   source ~/.bashrc
   # 启动splunk 第一次将需要设置admin密码。
   splunk start   
   ~~~

   其他linux环境安装，参考[Splunk Enterprise Linux Install](https://docs.splunk.com/Documentation/Splunk/8.0.1/SearchTutorial/InstallSplunk#Linux_installation_instructions)。

2. 创建一个index，用于本文的测试

    ~~~shell
    index=lab
    
    splunk add index $index \
      -homePath \$SPLUNK_DB/$index/db \
      -coldPath \$SPLUNK_DB/$index/colddb \
      -thawedPath \$SPLUNK_DB/$index/thawedDb
    ~~~

    如果index已经存在，可以用下面语句删除

    ~~~shell
    splunk remove index  $index
    ~~~

3. clone github repository。

    ~~~shell
    git clone git@github.com:xuxiangwen/splunk-study
    ~~~

    github repository中包含以下试验中所需要的数据和程序。

## 1. Data Input

数据输入，可以分为：

- Upload： 上传文件。一次性动作。
- Local Inputs： 在Splunk服务器上监控路径，或者开放端口。
- Forwarded inputs： 在日志产生的服务器，运行Forwarder，由Forwarder监控目录，或者开放端口，然后把数据传回Splunk。



### 1.1 Upload

上传有两种方式：

- 通过Splunk Web进行上传

- 通过Splunk CLI进行

  - oneshot

    ~~~
    splunk add oneshot -source ~/eipi10/user_profile_data/request_log/split/request_log_500000000_000.gz.05.tsv -index requestlog -sourcetype request_log.tsv
    ~~~

  - spool

    ~~~
    
    ~~~

    

### 1.2 Monitor





















## Reference

1. [Splunk Operational Intelligence Cookbook - Third Edition](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781788835237)