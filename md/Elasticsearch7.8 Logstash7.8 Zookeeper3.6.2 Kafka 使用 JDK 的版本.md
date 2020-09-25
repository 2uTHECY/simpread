\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[www.cnblogs.com\](https://www.cnblogs.com/kgdxpr/p/13323844.html)

**Elasticsearch 配置独立的 JDK**

Elasticsearch7.8 的 tar.gz 包中会提供一个 JDK 为：openjdk version "14.0.1"

如果全局已经设置了其他版本的 JDK，可以修改配置使用自带的 openjdk

修改 bin/elasticsearch-env 内容

```
if \[ ! -z "$JAVA\_HOME" \]; then
  JAVA="$JAVA\_HOME/bin/java"
  JAVA\_TYPE="JAVA\_HOME"
else
  if \[ "$(uname -s)" = "Darwin" \]; then
    # macOS has a different structure
    JAVA="$ES\_HOME/jdk.app/Contents/Home/bin/java"
  else
    JAVA="$ES\_HOME/jdk/bin/java"
  fi
  JAVA\_TYPE="bundled jdk"
fi

``` 

改为

```
JAVA="$ES\_HOME/jdk/bin/java"
JAVA\_TYPE="bundled jdk"

``` 

**注：如果没有全局的 JDK，不用修改配置 ES 可以直接启动**

**Logstash 配置独立的 JDK**

Logstash7.8 也使用 ES 的 openjdk 时，在启动过程中会有一些警告信息

如不想出现这些警告信息，可以换成 JDK1.8 版本

vi /usr/local/logstash/bin/logstash

export JAVA\_CMD="/usr/local/jdk1.8.0\_261/bin"　　# 改为自己 JDK 位置  
export JAVA\_HOME="/usr/local/jdk1.8.0\_261/"　　# 改为自己 JDK 位置

**Zookeeper 配置独立的 JDK**

修改 bin/zkEnv.sh

```
if \[\[ -n "$JAVA\_HOME" \]\] && \[\[ -x "$JAVA\_HOME/bin/java" \]\];  then
    JAVA="$JAVA\_HOME/bin/java"
elif type -p java; then
    JAVA=java
else
    echo "Error: JAVA\_HOME is not set and java could not be found in PATH." 1>&2
    exit 1
fi

``` 

改为

```
JAVA="/usr/local/jdk/bin/java"

``` 

**Kafka 配置独立的 JDK**

修改 bin/kafka-run-class.sh

```
if \[ -z "$JAVA\_HOME" \]; then
  JAVA="java"
else
  JAVA="$JAVA\_HOME/bin/java"
fi

``` 

改为

```
JAVA="/usr/local/jdk/bin/java"

``` 

\_\_EOF\_\_

![](http://dsideal-yy.oss-cn-qingdao.aliyuncs.com/C73336DC-19BE-11E8-87C1-00163E05EDE3.jpg)本文作者：**[缤纷世界 WB](https://www.cnblogs.com/kgdxpr/p/13323844.html)**  
本文链接：[https://www.cnblogs.com/kgdxpr/p/13323844.html](https://www.cnblogs.com/kgdxpr/p/13323844.html)  
关于博主：评论和私信会在第一时间回复。或者[直接私信](https://msg.cnblogs.com/msg/send/kgdxpr)我。  
版权声明：本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-nd/4.0/ "BY-NC-SA") 许可协议。转载请注明出处！  
声援博主：如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。您的鼓励是博主的最大动力！