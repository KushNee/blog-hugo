+++
title = "RT-AC66U_b1 中 smartdns 和 entware 冲突的有趣问题"
lastmod = 2019-12-20T23:45:44+08:00
categories = ["Tools"]
draft = false
author = "Kush Nee"
+++

今天在 RT-AC66U\_b1 上安装了 smartdns。安装过程本身没什么好说的，但是配置的时候发
现了一个有趣的问题。
<!--more-->

我根据项目说明准备写 conf 文件的时候，发现 `/opt/etc` 路径是不存在的，更不要说
`smartdns` 文件夹了。不过想着先照说明操作一遍，直接 `vi
/opt/etc/smartdns/smartdns.conf` ，然后重启路由器。 `nslookup` 的时候发现居然没
有使用我写的配置文件！为了测试，我特意把 `server-name` 改为了 `smartdns-custom`
，但是显示的仍然是 `smartdns` 。显然这说明因为没有读取到我的配置而使用了默认的。
经过了几次修改，不管是重启进程还是直接重启路由器都没有用，issue 里也没有类似的问
题。真是搞得我一脸懵逼。

然后喝杯咖啡冷静了一下，再次研究一下发现，虽然我重启进程是直接用的绝对路径，但是
系统启动的时候用的路径名其实是 `/opt/usr/sbin/smartdns` ，而且它也启动成功了。问
题在于 opt 下是没有 usr 路径的，它不可能凭空给我个东西啊。然后我回头看了下几个绝
对路径然后发现问题跟我想的完全不一样。

opt 是一个指向 tmp/opt 的软链接，而 tmp/opt 又是一个指向 smartdns 本身目录的软链
接，如此，启动命令才会生效。 `但是` ，entware 也有一个类似的将 opt 指向自身目录
的操作，这导致了，我创建的配置文件其实是在 entware 下的，自然不可能读取。

本来这个问题应该不存在的，可是 vi 可以直接创建路径，所以我用了全路径创建文件之后自动
给我创建了 entware 下的 etc/smartdns，想想也是挺好玩的。
