---
layout: post
title: "SonarQube,Jenkins,GitLab搭建.Net项目代码质量管理系统"
modified:
categories: 
description:
tags: []
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2015-10-28T14:45:44+08:00
---
## 前言
最近在负责部门内的代码质量审查相关工作，由于部门内开发人员的逐渐增多（其中很多人经验不足），每个开发人员的代码风格也不一致，虽有一些规范文档但并不能保证每个人都遵循。
最终造成的结果是GitLab上推送的都是百(乱)花（七）齐（八）放（糟）的代码，不符合规范的比比皆是，代码质量也参差不齐，最终所有的压力都落到了做代码审查的同事身上。
代码审查的同事需要花费很多时间去检查代码外观，而非业务逻辑。即使检查出来，开发人员也是极不情愿去修改，在多数初、中级程序员心里代码只要管用就行，没几个会关心统一的代码风格和质量。
软件的开发有80%时间是花在维护上，开发只占了20%。不关心代码风格和质量最终要么坑队友要么坑自己。为了不被自己和队友将来抱怨，我们急需一套系统来自动化检查代码质量。

## 说明
下面介绍的Sonar和Jenkins我们都是安装在windows Server 2008 R2上。

## SonarQube
SonarQube是一个开源的代码质量管理平台，用来分析和衡量源码的质量。分析的的语言包括：Java、PHP、C#、C、Cobol、PL/SQL、Flex 等，同时可以集成不同的测试工具，代码分析工具，以及持续集成工具。
我们下面会介绍SonarQube如何和Jenkins进行结合做到自动化分析代码。

## Jenkins
不用多说，Jenkins相信很多人都接触过，它是基于Java开发的一种持续集成工具，用于监控持续重复的工作。
我们下面会介绍如何在Windows上安装配置Jenkins。

## GitLab
  Gitlab是和GitHub差不多的版本管理系统，它是开源的。如果没有听过请直接关闭页面，去思考下是否还适合做程序员。

## 安装SonarQube
  下载地址：http://www.sonarqube.org/downloads/ 目前版本5.1.2
> SonarQube平台由三个组件组成
1. DataBase
用来保存SonarQube实例的一些配置（如安全，插件配置）
用来保存项目分析结果的快照
2. Web Server
用来供用户进行代码分析结果的查看和实例的配置
3. Analyzers
用来分析项目
其中只需配置一个DataBase和WebServer，但是有多个Analyzers，为了性能考虑，可以将DataBase和WebServer安装在一台机器，Analyzers安装在独立的机器上，不过Analyzers需要和DataBase处于同一网络，并且保持机器的时间同步

### 安装Sonar步骤
0. 确保机器上已经安装jre，如果想让后续性能更好点那就安装jdk和jre
1. 下载SonarQube，解压。（注意不要解压至数字开头的文件夹）
2. 配置数据库，Sonar支持多种DataBase，如MicroSoft SQL Server,MySQL,Oracle等，我们的系统是使用了mysql作为Sonar的DataBase。
首先创建Sonar数据库和Sonar用户，并给Sonar用户授权，管理mysql我们使用的是傻瓜式操作的Navicat for MySQL。
3. 修改SonarQube解压目录中conf文件夹下的sonar.properties文件
取消sonar.jdbc.username=sonar， sonar.jdbc.password=sonar，sonar.jdbc.url节点的注释
4. 进入sonarqube-5.1.2\bin\windows-x86-64 点击StartSonar
5. 打开浏览器，访问localhost:9000，登录（admin，admin）进行配置


