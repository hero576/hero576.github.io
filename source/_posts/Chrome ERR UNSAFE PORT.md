title: Chrome浏览器ERR_UNSAFE_PORT
author: hero576
tags:
  - web服务器
categories:
  - web服务器
date: 2018-03-11 13:26:00
toc: true
---
在开启flask项目时候使用了6000端口，谷歌就一直不能访问，出现下面的问题：

![upload successful](/images/pasted-7.png)

下面是Google Chrome 默认非安全端口列表，建议尽量避免以下端口：

| 端口号| 功能 |
| ------ |:----|
|1| tcpmux|
|7| echo|
|9| discard|
|11|systat|
|13|daytime|
|15|netstat|
|17|qotd|
|19|chargen|
|20|ftp data|
|21|ftp access|
|22|ssh|
|23|telnet|
|25|smtp|
|37|time|
|42|name|
|43|nicname|
|53|domain|
|77|priv-rjs|
|79|finger|
|87|ttylink|
|95|supdup|
|101|hostriame|
|102|iso-tsap|
|103|gppitnp|
|104|acr-nema|
|109|pop2|
|110|pop3|
|111|sunrpc|
|113|auth|
|115|sftp|
|117|uucp-path|
|119|nntp|
|123|NTP|
|135|loc-srv /epmap|
|139|netbios|
|143|imap2|
|179|BGP|
|389|ldap|
|465|smtp+ssl|
|512|print / exec|
|513|login|
|514|shell|
|515|printer|
|526|tempo|
|530|courier|
|531|chat|
|532|netnews|
|540|uucp|
|556|remotefs|
|563|nntp+ssl|
|587|stmp?|
|601|??|
|636|ldap+ssl|
|993|ldap+ssl|
|995|pop3+ssl|
|2049|nfs|
|3659|apple-sasl / PasswordServer|
|4045|lockd|
|6000|X11|
|6665|Alternate IRC [Apple addition]|
|6666|Alternate IRC [Apple addition]|
|6667|Standard IRC [Apple addition]|
|6668|Alternate IRC [Apple addition]|
|6669|Alternate IRC [Apple addition]|

