# 动态路由前端

## 项目描述

根据主机telnet连接网络设备的技术以及Python等技术，实现基于Web的网络拓扑搭建及校验的自动化。

本项目实现了一个拓扑，包括一台交换机以及三台路由器，主题为动态路由。

项目前端基于Vue搭建，Vue是一套用于构建用户界面的渐进式框架。

## 网络拓补

![Topography.png](https://i.loli.net/2021/01/08/lgf3pVIZxwBGokW.png)

## 运行环境

Vue.js with Vuetify

## 构建安装

需要启动“动态路由后端”项目，其余所有操作在浏览器客户端完成

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run server

# build for production with minification
npm run build
```

## 更新依赖

使用npm-check-updates帮助我们检查版本是否有变化

> 更新依赖可能会导致项目失效，请谨慎执行

``` bash
# 安装
npm install -g npm-check-updates

# 检查当前目录下可更新的依赖项
ncu

# 升级 package.json
ncu -u

# 根据更新的 package.json 安装新版本
npm install
```
