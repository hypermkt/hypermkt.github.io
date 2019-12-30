---
layout: post
title: Mac(Mavericks)にJenkinsをインストールして起動する方法
post_id: 330
categories: 
- Jenkins
---

JenkinsはJavaで書かれた継続的インテグレーションツールです。Jenkinsを利用する事でテストやデプロイなど日々の作業を自動化することができます。MacでJenkinsを試したい場合は簡単にインストールする事が出来すぐに利用できるのでインストール＆起動方法をまとめます。##前提



ソフトウェア
  
バージョン

OSX Mavericks
  
10.9.3

Java
  
Java(TM) SE Runtime Environment (build 1.8.0_11-b12)

Homebrew
  
0.9.5


## インストール



### ターミナルより以下コマンドを実行



brew install jenkins


### Jenkinsを起動する



java -jar /usr/local/opt/jenkins/libexec/jenkins.war