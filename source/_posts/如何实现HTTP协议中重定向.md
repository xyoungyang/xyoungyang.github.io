---
title: 如何实现http协议中的重定向？
date: 2019-04-22 14:47:40
categories: [java web]
tags: 
	- 重定向
	- 实现原理
---
我是一个新的java学习爱好者，业余的时候写一些简单的技术博客供大家一起学习交流，欢迎大家互相学习，共同进步。如果有什么不正确或者有什么建议欢迎指教！！
<!--more-->
在学java web的时候遇到一个重定向的问题。

首先讲解下什么是重定向：在 HTTP 协议中，重定向操作由服务器通过发送特殊的响应（即 redirects）而触发。HTTP 协议的重定向响应的状态码为 3xx 。浏览器在接收到重定向响应的时候，会采用该响应提供的新的 URL ，并立即进行加载；大多数情况下，除了会有一小部分性能损失之外，重定向操作对于用户来说是不可见的。
	
简单的说就是：客户端向服务器发送请求，但是服务器的某个资源完成不了这个请求，让客户端去请求另一个资源来完成请求。就是一个请求转发的过程。

我是采用IntelliJ IDEA来完成的，首先先new两个Servlet，在这里实现httpServlet，起名responseDemo1和responseDemo2。

	其实重定向有两种方式：
	第一种：
 1、设置状态码为302：response.setStatus(302);
 2、设置响应头的location，设置请求资源路径 response.setHeader("location","资源路径");
   第二种 ：
   直接调用response.sendRedirect("资源路径")

	第一个Servlet代码如下：
	`package cn.itcast.servlet;
	
	import javax.servlet.ServletException;
	import javax.servlet.annotation.WebServlet;
	import javax.servlet.http.HttpServlet;
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	import java.io.IOException;

	@WebServlet("/responseDemo1")
	public class responseDemo1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws 		ServletException, IOException {
        System.out.println(request.getMethod());
        System.out.println("responseDemo1.....");
       /* //设置状态码302
        response.setStatus(302);
        //设置资源路径
        response.setHeader("location", "/day15/responseDemo2");*/
       response.sendRedirect("/day15/responseDemo2");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request,response);
    }
}
`
	第二个如下：
	
	`	package cn.itcast.servlet;

	import javax.servlet.ServletException;
	import javax.servlet.annotation.WebServlet;
	import javax.servlet.http.HttpServlet;
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	import java.io.IOException;

	@WebServlet("/responseDemo2")
	public class responseDemo2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println(request.getMethod());
        System.out.println("responseDemo2.......");
    }

	 protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request,response);
    }
}
`
