# Solr使用步骤

solr-7.2.1.tgz，solr需要先有java环境才能启动成功

### 简介

Solr：开源搜索平台，用于构建搜索应用。组件应用，企业级、开源、高度可扩展。

**可扩展的，可部署的企业级的开源搜素引擎。优化大量以文本为中心的数据。**

### 一、Solr安装

1. 解压到 usr/local

2. 建议改名为solr：mv  solr-7.2.1/  solr
3. 启动：进入bin目录执行 ./solr  start  -force
4. 开放端口号：firewall  -cmd  --add-port=8983/tcp  --permanent
5. 重启防火墙：firewall  -cmd --reload
6. 在windows浏览器上访问：ip地址:8983
7. 创建Solr集合命令：./solr  create  -c  collection1  -force（前提：solr是启动的、命令在bin目录执行）



### 二、导入分词器

导入ik分词器(能比较好的支持中文)

1. 下载地址：https://github.com/EugenePig/ik-analyzer-solr5
2. 解压到某个目录，进入解压后的ikxxx-master目录
3. shift+右键，打开powshell界面，输入指令：mvn  clean  install，将其打成jar包
4. 去maven仓库(执行指令后路径会显示出来)找到这个jar包
5. 将jar包传进linux上 **/usr/local/solr/server/solr-webapp/webapp/WEB-INF/lib **目录下
6. 将github上的Configuration下的具体配置复制进**/usr/local/solr/server/solr/collection1/conf/managed-schema**文件里
7. 重启Solr：./solr  restart  -force
8. 在windows浏览器上访问：ip地址:8983，在Analysis下就能找到这个分词器了



### 三、域配置

在**/usr/local/solr/server/solr/collection1/conf/managed-schema**文件里加入对应配置

**1.普通域**

name：给字段取名，type：该字段的类型，indexed：是否允许索引，stored：是否允许存储

```
<field name="item_goodsId" type="plong" indexed="true" stored="true"/>
<field name="item_title" type="text_ik" indexed="true" stored="true"/>
<field name="item_price" type="pdouble" indexed="true" stored="true"/>
<field name="item_image" type="string" indexed="true" stored="true"/>
<field name="item_category" type="string" indexed="true" stored="true"/>
<field name="item_brand" type="string" indexed="true" stored="true"/>
<field name="item_seller" type="text_ik" indexed="true" stored="true"/>
<field name="item_updatetime" type="pdate" indexed="true" stored="true"/>
```

**2.复制域**

multiValued：是否允许多选，多值同时搜索，source：要复制的字段
```
<field name="item_keywords" type="text_ik" indexed="true" stored="false" multiValued="true"/>
<copyField source="item_title" dest="item_keywords"/>
<copyField source="item_category" dest="item_keywords"/>
<copyField source="item_seller" dest="item_keywords"/>
<copyField source="item_brand" dest="item_keywords"/>
```

**3动态域**

```
<dynamicField name="item_spec_*" type="string" indexed="true" stored="true"/>
```