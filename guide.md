### 数据处理

- Index-time Processing：是Splunk从source种读取数据的过程，包括如下动作：
  - 识别sourcetype
  - 抽取timestamp
  - 抓取event
  - 把event写入index
- Search-time Processing：search满足需要的event，进行整理加工，然后输出的过程。

### Events

An event is a single entry of data.  More specifi cally,  an event is a set of values associated with a timestamp.

一个event是单个数据条目，具体来说，是和时间相关的一组数据。

### Source

A source is the name of the fi le, stream, or other input from which a particular event originates 

Source指数据源的名字。数据源可以是文件，stream，或者其它输入。

### Sourcetype

根据数据的格式，把Source分成不同的Sourcetype。相同sourcetype的event可以来自不同的source.

### Host

A host is the name of the physical or virtual device where an event originates. 

Host指产生event的物理或者虚拟设备。

### Index

一组文件，分为两种：

- Event Index：默认的index是“main” Index。
- Metric Index

### Field

Fields are searchable name/value pairings in event data. 

字段。

### Tag

Tags are aliases to field values. For example, if there are two host names that refer to the same computer, you could give both of those host values the same tag (e.g., "hall9000"), and then if you search for that tag (e.g., "hal9000"), Splunk will return events involving both host name values.

别名。

### Eventtype

Eventtypes are cross-referenced searches that categorize events at search time. 

还是有点儿不理解

### Splunk开发步骤

首先要明确，需要要回答什么问题（the questions you want to answer），比如：

- Why did that system fail?
- Why is it so slow lately? 
- Where are people having trouble with our website?

接下来，可以分成三个步骤。

- Can the data provide the answers? 找到哪些数据能够回答这些问题。
- 把数据转化成适当的结果，这些结果可以回答这些问题。
- 把答案通过report, chart, graph展现出来。以便更多的用户可以很好的理解。

![image-20200109144804666](images/image-20200109144804666.png)

### Directory Structure

~~~
splunk
├── bin : executables
│   ├── jars
│   └── scripts
├── etc : licenses, configrations
│   ├── apps
│   ├── system
│   └── users
├── include
│   ├── python2.7
│   └── python3.7m
├── lib
└── var
    ├── lib
    │   └── splunk : splunk_db, 也就是indexes
    |	    ├──defaultdb/
    |            ├── colddb
    |            ├── datamodel_summary
    |            ├── db
    |            │   ├── db_1575887042_1575216902_0
    |            │   │   └── rawdata
    |            │   ├── db_1578133442_1577463301_1
    |            │   │   └── rawdata
    |            │   ├── db_1578326385_1575666302_2
    |            │   │   └── rawdata
    |            │   └── GlobalMetaData
    |            └── thaweddb	: 解冻的
    ├── log
    ├── run
    └── spool

~~~

![image-20200108160854510](images/image-20200108160854510.png)

### Core Component

![image-20200109125316230](images/image-20200109125316230.png)

- Indexer： An indexer provides indexing capability for local and remote data.
  
- Search Head： 把search reqeust分发到Indexer，然会汇总Indexer返回的结果，然后进行一定的加工。
  - Dashboard
  - Report
  - Visualization
  
- Forwarder： 收集数据。

  A forwarder is a version of Splunk that allows you to send data to a central Splunk indexer or group of indexers. 

- License Server

- Deployer

- Cluster Master

![image-20200108151839236](images/image-20200108151839236.png)



### optimize search

The key to fast searching is to limit the data that needs to be pulled off disk to an absolute minimum, and then to filter that data as early as possible in the search so that processing is done on the minimum data necessary.

search优化的关键思路：使得从磁盘读取的数据尽量的小。具体的方式有；

- 把数据分散到多个index。
- 明确search内容。Search as specifi cally as you can (e.g. fatal_error, not *error*)
- 增加time range
- 移除不需要的field
- 先过滤数据，然后计算
- Report
  - use the Advanced Charting view, and not the Flashtimeline view, which calculates timelines.
  - On Flashtimeline, turn off ‘Discover Fields’ when not needed.

- 使用summary index. summary index对数据进行了pre-calculate。
- 磁盘的IO尽可能的快







## 参考

- Exploring Splunk:  https://www.splunk.com/pdfs/ebooks/exploring-splunk.pdf

  12 page

- Quick Reference Guide:  https://docs.splunk.com/images/1/17/4.2.x_search_language_refcard.pdf



