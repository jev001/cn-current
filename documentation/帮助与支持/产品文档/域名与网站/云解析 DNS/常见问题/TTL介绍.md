## **TTL介绍**

## **一、什么是域名的 TTL 值？**

TTL是域名解析的生命周期，TTL值全称是“生存时间（Time To Live)”，简单的说它表示DNS记录在DNS服务器上的缓存时间。每一次访问网站A，不会每次都到DNS服务器域名解析，而是第一次访问时才到DNS服务器进行解析，然后解析的结果会缓存到当地的递归DNS服务器上，当地的第二个用户访问网站A时，递归服务器会直接返回解析结果，而不会再向DNS服务器请求解析，那么多久之后递归服务器才会更新这个解析结果呢？这就是TTL来决定的。

例如：

有一个域名www.aaa.com，对应IP地址为1.1.1.1，把它的TTL设为3600秒，这条记录存储在一台DNS服务器上。现在有一个用户在访问www.aaa.com时，网络服务商的DNS就会试图为用户解析www.aaa.com，当然网络服务商这台DNS服务器并没有包含www.aaa.com这条信息，因此无法立即解析，但是通过全球DNS的递归查询后，最终定位到www.aaa.com这台DNS服务器对应的IP地址为1.1.1.1并将结果告诉网络服务商的DNS服务器，然后再由网络服务商告诉用户结果。网络服务商为了以后加快对www.aaa.com这条记录的解析，就将刚才的1.1.1.1结果保留一段时间，这段时间就是TTL值，在这段时间内如果用户又有对www.aaa.com这条记录的解析请求，它就直接告诉用户IP地址为1.1.1.1，当TTL到期则又会重复上面的过程。

## **二、TTL通常设置为多少合适？**

如此看来，那我把TTL设置为非常小，比如1秒，岂不是最好，这样我修改了解析那么对于用户来说立即就可以生效。答案是否定的，如果TTL设置为1秒，那么就意味着几乎每次用户的解析，递归服务器都需要向DNS服务器进行解析请求，这样所耗费的时间就会增加很多，而且权威服务器的解析因为要判断用户的来源进行智能解析，比起来直接使用缓存回答耗费的时间会更长，而且失败率也会更高，因此访问体验和解析稳定性都有损害。

当前京东云界面TTL值可以设置范围是60-604800秒。

下表给出一些建议供您参考：
IP经常变动

动态IP

宕机检测

服务器架构

建议TTL值（秒）否

否

不使用

单服务器

600否

否

使用

多服务器

180是

否

不使用

单服务器

300是

是

不限

不限

120否

否

是

大型网站

60否

否

否

热备、容灾、IP固定

3600