---
title: "node.jsをバージョン管理 (nodebrew使用)"
date: 2018-06-17T03:40:35+09:00
draft: false
description: "node.jsをnodebrewを使ってバージョン管理します。"
categories: ["develop"]
tags : ["node.js","nodebrew"]
archives: ["2018"]
---

プロジェクト毎にnode.jsのバージョンが違う場合があります。  
切り替えが簡単にできれば。。。  
そのような時にはnodebrewを使えば簡単にバージョン管理ができます。 

## 1. 前準備 

### 1.1. node.js 削除
もしnode.jsがインストールされている場合、下記のコマンドで削除をします。  
なお、ターミナル上で一行づつコピー&ペーストをして実行。  

```bash
lsbom -f -l -s -pf /var/db/receipts/org.nodejs.node.pkg.bom \
| while read i; do
  sudo rm /usr/local/${i}
done
sudo rm -rf /usr/local/lib/node \
     /usr/local/lib/node_modules \
     /var/db/receipts/org.nodejs.*
```  
\- 参考にさせていただきました -  
・[MacにpkgでインストールしたNode.jsをアンインストールする手順](http://qiita.com/yoshikoba/items/4906829faaaae8c73e56)


## 2. nodebrew導入

### 2.1. インストール
homebrewは使用せずにインストールします。    
コマンドは[nodebrewのgitページ](https://github.com/hokaccha/nodebrew)の通り。  

```bash
$ curl -L git.io/nodebrew | perl - setup
```  

### 2.2. 環境変数設定
インストール完了時に表示された記述を参考に環境変数を設定します。  

```bash
Installed nodebrew in $HOME/.nodebrew

========================================
Export a path to nodebrew:

export PATH=$HOME/.nodebrew/current/bin:$PATH
========================================
```  
ファイルは、`~/.bashrc`(もしくは`~/.bash_profile`)。  

```bash
$ vi ~/.bashrc
```  

設定を適用し確認。  

```bash
$ source ~/.bashrc 
$ nodebrew help
nodebrew 1.0.0

Usage:
    nodebrew help                         Show this message
    nodebrew install <version>            Download and install <version> (from binary)
    nodebrew compile <version>            Download and install <version> (from source)
    nodebrew install-binary <version>     Alias of `install` (For backword compatibility)
    nodebrew uninstall <version>          Uninstall <version>
    nodebrew use <version>                Use <version>
    nodebrew list                         List installed versions
    nodebrew ls                           Alias for `list`
    nodebrew ls-remote                    List remote versions
    nodebrew ls-all                       List remote and installed versions
    nodebrew alias <key> <value>          Set alias
    nodebrew unalias <key>                Remove alias
    nodebrew clean <version> | all        Remove source file
    nodebrew selfupdate                   Update nodebrew
    nodebrew migrate-package <version>    Install global NPM packages contained in <version> to current version
    nodebrew exec <version> -- <command>  Execute <command> using specified <version>

Example:
    # install
    nodebrew install v8.9.4

    # use a specific version number
    nodebrew use v8.9.4
```  


## 3. nodebrew使用

### 3.1. node.js一覧表示
インストール可能なnode.js、    
ローカルにインストールされているnode.js、  
現在インストールされているnode.jsを表示させます。  

```
$ nodebrew ls-all
```  

表示結果は以下。

```bash
remote:
(省略)
v10.0.0   v10.1.0   v10.2.0   v10.2.1   v10.3.0   v10.4.0   v10.4.1   
(省略)

local:
not installed

current: none
```  

### 3.2. node.jsインストール
ローカル環境にnode.jsをインストールします。  

```bash
$ nodebrew install-binary stable
Fetching: https://nodejs.org/dist/v10.4.1/node-v10.4.1-darwin-x64.tar.gz
######################################################################## 100.0%
Installed successfully
```  
\* stableは安定板、latestとすると最新版  

### 3.3. node.jsをローカル環境に適用
このままではnode.jsは使えない為、適用させる必要があります。  

```bash
$ nodebrew ls
v10.4.1

current: none
$ nodebrew use v10.4.1
$ node -v
v10.4.1
```  
<br>
以上で、node.jsのバージョン切替えができるようになりました。