# flink集群搭建

- 摘自 https://blog.csdn.net/zhou_shaowei/article/details/76258240

## 下载flink
由于我没有使用haddop所以下载无hadoop的flink版本：flink-1.5.0-bin-scala_2.11.tgz
http://www.apache.org/dyn/closer.lua/flink/flink-1.5.0/flink-1.5.0-bin-scala_2.11.tgz

## 环境准备
- 准备三台服务器(192.168.1.60,192.168.1.62,192.168.1.63)
- 搭建zookeeper集群
- 规划一台master(192.168.1.60)和两台slave(192.168.1.62,192.168.1.63)，并且配置master到slave的ssh免密登录

## 配置master节点
选择一个 master节点(JobManager)然后在conf/flink-conf.yaml中设置jobmanager.rpc.address
配置项为该节点的IP 或者主机名。确保所有节点有有一样的jobmanager.rpc.address 配置。

```
192.168.1.60:8081
```
## 配置slaves节点
将所有的 worker 节点 （TaskManager）的IP 或者主机名（一行一个）填入conf/slaves 文件中。

```
1921.68.1.62
192.168.1.63
```
## 配置zookeeper
在conf/zoo.cfg中配置正确的zookeeper地址

```
clientPort=12181

# ZooKeeper quorum peers
server.1=192.168.1.60:12888:13888
server.2=192.168.1.62:12888:13888
server.3=192.168.1.63:12888:13888

```

## 启动集群
- 在master节点的flink目录下运行/bin/start-cluster.sh

## Http接口
flink提供了http api可以远程进行相关功能操作，例如可以上传一个jar，启动一个job，获取job详情，获取所有job列表等等
具体api参考flink官方文档
https://ci.apache.org/projects/flink/flink-docs-master/monitoring/rest_api.html

一下给出上传jar，运行job，获取job三个例子代码

```
/**
   * 上传一个jar包
   * @param fileName 文件名称（jar包完整路径）
   * @return jarId
   */
  public    String upload(String fileName){
      logger.info("upload:{}",fileName);
      String url = "http://"+flinkServer+"/jars/upload";
      Request req = Request.create(url, Request.METHOD.POST);
      File f = new File(fileName);
      Map params =   req.getParams();
      params.put("file", f);
      Map map = new HashMap<>();
      map.put("Content-Disposition","Content-Disposition: form-data; name=\"jarfile\"; filename=\""+f.getName()+"\"");
      map.put("Content-Type","application/x-jara-archive");
      req.setHeader(Header.create().addAll(map));
      FilePostSender sender = new FilePostSender(req);
      Response resp = sender.send();
      String json = resp.getContent();
      Map result = (Map) Json.fromJson(json);
      if("success".equals(result.get("status"))){
          logger.info("upload:{} success",fileName);
          String jar = result.get("filename").toString();
          String jarId =jar.split("flink-web-upload/")[1];
          return jarId;
      }else{
          logger.info("upload:{} error:{}",fileName,json);
      }
      return "";
  }

  /**
   * 启动一个job
   * @param jarId  jarId
   * @param entryClass 入口类
   * @return { "jobid": "1452a2e3a02eb3d890dcecd10a563f8d"}
   */
  public    Map run(String jarId,String entryClass){
      logger.info("run job,jarId:{},entryClass:{}",jarId,entryClass);
      String url = "http://"+flinkServer+"/jars/"+jarId+"/run?entry-class="+entryClass;
      Map<String, Object> parms = new HashMap<String, Object>();
      String response = Http.post(url,
              parms,
              5 * 1000);
      logger.info("run job,jarId:{},entryClass:{},result:{}",jarId,entryClass,response);
      return (Map) Json.fromJson(response);

  }

  /**
   * 获取job详情
   * @param jobId
   * @return
   */
  public Map getJob(String jobId){
      logger.info("get Job detail,jobId:{}",jobId);
      Response response = Http.get("http://"+flinkServer+"/jobs/"+jobId);
      return (Map) Json.fromJson(response.getContent());
  }
```
## 运维

flink运行过程，如果有什么异常可以通过其log目录下的日志进行查看分析,如下所示最新运行日志为：flink-flink-standalonesession-0-h1.log

```
-rw-rw-r-- 1 flink flink 406790609 5月  29 22:17 flink-flink-standalonesession-0-h1.log
-rw-rw-r-- 1 flink flink     17820 5月  28 02:51 flink-flink-standalonesession-0-h1.log.1
-rw-rw-r-- 1 flink flink     11874 5月  28 02:47 flink-flink-standalonesession-0-h1.log.2
-rw-rw-r-- 1 flink flink    188854 5月  28 02:42 flink-flink-standalonesession-0-h1.log.3
-rw-rw-r-- 1 flink flink         0 5月  28 02:52 flink-flink-standalonesession-0-h1.out
-rw-rw-r-- 1 flink flink         0 5月  28 02:47 flink-flink-standalonesession-0-h1.out.1
-rw-rw-r-- 1 flink flink         0 5月  28 02:44 flink-flink-standalonesession-0-h1.out.2
-rw-rw-r-- 1 flink flink         0 5月  27 18:34 flink-flink-standalonesession-0-h1.out.3
-rw-rw-r-- 1 flink flink      7133 5月  27 18:53 flink-flink-standalonesession-1-h1.log
-rw-rw-r-- 1 flink flink         0 5月  27 18:53 flink-flink-standalonesession-1-h1.out
-rw-rw-r-- 1 flink flink      7133 5月  27 19:02 flink-flink-standalonesession-2-h1.log
-rw-rw-r-- 1 flink flink         0 5月  27 19:02 flink-flink-standalonesession-2-h1.out
-rw-rw-r-- 1 flink flink      7133 5月  27 19:03 flink-flink-standalonesession-3-h1.log
-rw-rw-r-- 1 flink flink         0 5月  27 19:03 flink-flink-standalonesession-3-h1.out

```
