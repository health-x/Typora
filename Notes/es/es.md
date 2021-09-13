springboot操作es

1.导入依赖：

```xml
<!--elasticsearch-->
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
</dependency>
```

2.编写代码

```java
//1.获取连接的客户端
private RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(
                HttpHost.create("http://47.100.81.153:9200")
//2.构建请求

//3.执行

//4.获取结果

//5.关闭连接
client.close();
```

