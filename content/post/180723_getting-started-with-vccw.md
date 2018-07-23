---
title: "VCCWを導入"
date: 2018-07-23T13:27:24+09:00
draft: true
description: "案件でWordpressを使用する為、VCCWを導入しました。"
categories: ["develop"]
tags: ["VCCW","Wordpress"]
archives: ["2018"]
---

## VCCWとは
Vagrantを使用したWordpress開発環境です。  

## VCCW導入
[公式サイト](http://vccw.cc/)を確認しつつ、導入作業をしていきます。  

### VirtualBox,Vagrant導入
下記URLからファイルをDLして導入します。
・[Virtualbox](https://www.virtualbox.org/)  
・[Vagrantup](https://www.vagrantup.com/)  

### vagrant-hostsupdater導入
ターミナルから`vagrant-hostsupdater plugin`をインストールします。  

```bash
$ vagrant plugin install vagrant-hostsupdater
Installing the 'vagrant-hostsupdater' plugin. This can take a few minutes...
Fetching: vagrant-hostsupdater-1.1.1.160.gem (100%)
Installed the plugin 'vagrant-hostsupdater (1.1.1.160)'!
```
尚、`vagrant-hostsupdater plugin`とは、、、  

> このプラグインは、Vagrant を起動するときに、ホスト（ここでは Mac ）の /etc/hosts を Vagrantfile に記述したものに設定してくれます。仮想マシンを作成するたびに、/etc/hosts を開いて、ドメインや ip アドレスを設定しなくてもよくなります。  
> [Vagrant のプラグイン vagrant-hostsupdater をインストールする](http://stangler.hatenablog.com/entry/2016/10/04/162244)  

### box追加
ターミナルからvagrantのboxを追加します。  

```bash
$ vagrant box add vccw-team/xenial64
```  

\* 下記エラーが表示され追加ができない場合がありましたが、
もう一度同じコマンドで実行し問題なく追加が完了しました。

```bash
An error occurred while downloading the remote file. The error
message, if any, is reproduced below. Please fix this error and try
again.

OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 60
```  

エラーの詳細は下記に。  

> SSL_ERROR_SYSCALL は何かしらの I/O エラーが起こったとき、特にソケット関係のエラーが起こったときに出るエラーです。更に macOS において errno 60 はタイムアウトを意味します。つまりこのエラーはソケット I/O のタイムアウトが原因と考えられます。  
> [vagrant - vagrantのboxのダウンロードが出来ない: SSL_ERROR_SYSCALL, errno 60 - スタック・オーバーフロー](https://ja.stackoverflow.com/questions/42546/vagrant%E3%81%AEbox%E3%81%AE%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%89%E3%81%8C%E5%87%BA%E6%9D%A5%E3%81%AA%E3%81%84-ssl-error-syscall-errno-60)  

### VCCW導入
[こちら](http://vccw.cc/)でVCCWをダウンロードし、解凍後ターミナルでそのディレクトリに移動します。  

### VCCW起動
`$ vagrant up`でVCCWを実行します。  


起動後に下記へアクセスすると、Wordpressトップを表示できるようになります。(デフォルト設定。)

・ [http://vccw.test/](http://vccw.test/)  
・ [http://192.168.33.10/](http://192.168.33.10/)  