---
layout: post
title: 'i2c_smbus_write_byte_data was not declared'
key: 20180627
category: Qt
tags:
- Document
- Ubuntu
- Qt
---

Ubuntu上編譯I2C相關程式時，跳出錯誤 **i2c_smbus_write_byte_data** was not declared

<!--more-->

打開Terminal敲入下面指令，安裝缺乏的Library即可。
```
sudo apt-get install libi2c-dev
```
