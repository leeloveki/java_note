# Spring Boot

SpringBoot框架基于Spring4.0设计, 在Spring的基础上将配置进行简化, 实现了约定大于配置(Convention over configuration)的思想

约定大于配置: 由SpringBoot自动配置框架, 开发者仅需要提b供必要信息的思想, 该特性降低了开发的灵活性, 但是可以降低开发者的配置工作量, 并实现代码编译测试打包的自动化

SpringBoot通过Parent版本方案对依赖进行集成方案管理, 开发者只需要提供列出需要使用的依赖坐标(名称), 框架会根据parent方案的版本自动选择依赖的版本

> parent实现了整个项目依赖的版本统一管理

SpringBoot可以集成大量的框架, 并对框架提供了开箱即用(OutOfBox)(自动化配置)功能

SpringBoot特点:

1. 有内置的Servlet容器可以独立启动, 不依赖于外部Tomcat容器, 启动速度快
2. 提供各种starters(parent版本方法)来简化Maven依赖配置
3. 对大量框架实现了自动化配置, 简化了程序员的工作
4. 提供了大量的预定义特性, 可以从外部监控程序的特性(监控, 效率)