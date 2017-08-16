* dnslog


## 注入
get 型注入
post 型注入
cookie 注入
或者
int型注入
字符型注入
宽字节注入
order by注入
报错注入
时间盲注注入
bool盲注注入

报错函数
xmlupdate
extructvalue
floor
exp

字符串拼接函数
left
mid
substring
right

getshell
读文件 load_file()
写文件 into outfile or into dumpfile

绕过
代替‘ ’ 可以用%0a,%0b,'+'和一些内连注释
过滤等号，可以用like代替
过滤逗号，可以用join来过滤
过滤关键字，大小写绕过，双写绕过

## 上传

## xxe

## 序列化

## xss
xss 平台
beef
常见绕过
跨域方法

## 提权
exp
为授权访问
linux下crontab
mysql udf提权

## csrf 
接触的较少，配合重定向漏洞可发挥大的效果

## 信息收集
whois 域名信息
aizhan.com 域名信息

## 代理

### 正向代理
ew
proxychains
### 反向代理
ssh
ew
lcx
regeorg

## 常用工具
msfconsole
sqlmap
awvs
openvas
nmap
burpsuite
御剑扫后台

## ddos

dns放大攻击：通过伪造源地址查询dns，致使dns返回大量数据给被攻击ip
snmp放大攻击：
udp攻击：
tcp攻击：
c&c攻击：

应用层攻击 syn flood，ping of death，arp,dns,802.11.ssl
网络层攻击：icmp flood，udp flood
协议层攻击
