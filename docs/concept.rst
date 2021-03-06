.. concept .

.. contents:: 本章目录
  :depth: 2

-----------
概念简介
-----------

Nano平台目前包含三个模块：Core/Cell/FrontEnd

Cell负责云主机的创建与管理；Core将多个Cell组成资源池，根据需求在池内调度和分配云主机；FrontEnd调用Core的API接口为用户提供HTML5的管理门户。

所有模块可以安装在一个服务器上，作为All In One平台进行体验和测试，但是对生产环境部署时，为了保障平台可用性，建议每个模块都部署在独立服务器上，如下图所示：

.. image:: images/1_1_nano_modules.png

Nano平台的网络通讯分为外部和内部两部分，外部通讯目前主要是Web管理端和Core API端口，用于用户访问和应用调用；默认端口Web为TCP 5850，API为TCP 5870，用户可以根据自己环境进行调整。

内部通讯主要是模块间的UDP协议和数据传输用HTTPS协议，协议端口都是平台自行动态分配和管理，通常情况下，管理员无需配置。

.. image:: images/1_2_communicate_overview.png


通讯域
==========

Nano集群的模块可以相互发现，自动完成组网和识别，无需管理员配置。

Nano的自动发现基于组播协议实现，Nano通过 <通讯域名称:组播地址:组播端口> 的三元组定义一个独立的通讯域（默认为<"Nano":224.0.0.226:5599>），同一通讯域内模块可以相互发现、识别和通讯。

如果需要在一个局域网内配置多个Nano集群，可以通过分配不同的通讯域地址来进行区分，有效的组播地址为224.0.0.0～224.0.0.255，具体请参考 `Multicast address <https://en.wikipedia.org/wiki/Multicast_address>`_ 。

工作原理如图：

.. image:: images/1_3_domain_discovery.png

**Core作为接听模块，应该最先启动。** 如果Core停止服务或者重启，已经启动的模块会自动尝试找回Core服务并重新加入通讯组。

资源模型
==========

一个Nano本地集群构成一个可用域(Zone)，一个域包含多个资源池(Pool)，每个资源池包含一个或者多个Cell资源节点。

一个Cell只能属于一个Pool，当用户请求创建或者迁移云主机时，Core根据指定资源池内各Cell的实时负载，选择一个合适的Cell进行云主机实例创建。

.. image:: images/1_4_resource_model.png

Nano平台搭建完成后，会有一个空的Default资源池，在尝试创建云主机之前，请记得 **首先往资源池中添加一个可用的Cell节点** 。

镜像
========

为了便于云主机部署和维护，Nano提供了两种镜像：磁盘镜像和光盘镜像。

磁盘镜像保存云主机的系统盘数据，用户可以通过磁盘镜像快速复制新的云主机实例，直接获得与模板云主机相同的操作系统和预装软件，大幅度提高批量部署示例的效率。

光盘镜像保存了ISO格式的光盘数据，用于加载到云主机中安装操作系统或者其他系统软件，通常用于定制模板云主机。

你可以直接将准备好的镜像上传到平台中并开始使用，节省制作模板的时间。可以访问Nano官方网站 `下载 <https://nanos.cloud/zh-cn/download.html>`_ 页面，获取预置的CentOS 7镜像。

.. image:: images/1_5_images_overview.png


----

了解了Nano的基本概念，就可以开始进行平台的部署与安装了
