---
title: "node.jsをバージョン管理 (ndenv使用)"
date: 2018-06-17T03:46:36+09:00
draft: false
description: "node.jsをndenvを使ってバージョン管理します。"
categories: ["develop"]
tags : ["node.js","ndenv"]
archives: ["2018"]
---

nodebrewを使ってnode,jsを管理していましたが、  
案件毎にコマンド打つのも面倒に。。  
それに案件で使用しているバージョンを確認するのも大変でした。  

なんてことを考えていたら、ndenvというツールがあることを知りました。  
ディレクトリ毎(案件毎)にバージョンを自動的に変更してくれるスグレモノでした。  


## 1. 前準備 (nodebrew削除)

### 1.1. 既存環境確認
念のため、削除前に既存環境で使用していたnodeを確認しました。  
(必要？)  

```bash
$ nodebrew ls
v5.12.0
v6.10.3
v7.9.0
v8.2.1
v8.8.1
v9.2.0
v9.5.0
```  

### 1.2. 環境変数削除
```bash
  vi ~/.bashrc
```  
下記を削除  
```
export PATH=$HOME/.nodebrew/current/bin:$PATH:
```  
### 1.3. nodebrew削除
ホームディレクトリ配下にある`.nodebrew`を削除。  

### 1.4. nodebrew削除確認
```bash
$ nodebrew -v
-bash: /Users/<ユーザー>/.nodebrew/current/bin/nodebrew: No such file or directory
```  


## 2. ndenv導入

### 2.1. インストール
homebrew経由でndenvをインストールします。  

```bash
$ brew install ndenv
```  

### 2.2. 環境変数追記
`~/.bashrc`(もしくは`~/.bash_profile`)を修正し、 　
環境変数を設定します。  

```bash
  vi ~/.bashrc
```  

追記内容は下記  

```
export PATH="$HOME/.ndenv/bin:$PATH"
eval "$(ndenv init -)"
```  

変更内容を適用させます。  

```bash
source ~/.bashrc
```  

### 2.3. node-build導入
ndenvはインストールできましたが、  
このままでは肝心の`ndenv install`が実行できません。  
その為、node-buildをインストールします。  

```bash
git clone https://github.com/riywo/node-build.git $(ndenv root)/plugins/node-build
```  

## 3. ndenv使用
### 3.1. node.jsリストの表示
インストール可能なnode.jsリストを表示させる  

```bash
$ ndenv install -l
```  

### 3.2. node.jsインストール  
希望のバージョンのnode.jsをインストールします。  

```bash
$ ndenv install [version]
```  

### 3.3. グローバルでのバージョンを指定
ローカルでデフォルトで使用するバージョンを指定します。  

```bash
$ ndenv global [version]
$ ndenv rehash
```  

グローバル設定は`~/.ndenv/versions/`ディレクトリを参照している。  
その為、設定を解除する為には、`~/.ndenv/versions/`を削除すれば良い。  

```bash
$ ll ~/.ndenv/versions/
total 0
drwxr-xr-x   3 ******  staff  102  6  1 11:16 .
drwxr-xr-x   5 ******  staff  170  6  1 11:03 ..
drwxr-xr-x  10 ******  staff  340  6  1 11:19 v8.11.2
```
### 3.4. ローカルでのバージョンを指定
下記を実行することでディレクトリ毎にバージョンを設定できます。  

```bash
$ ndenv local [version]
```  

実行するとディレクトリ内に`.node-version`ファイルが作成されます。  
ローカルでの指定を解除する為には、このファイルを削除します。  

```bash
$ cat .node-version
v8.11.2
```  

### 3.5. 実行結果
例として、下記の環境を作りました。  

- グローバルではv8.11.2を指定
- Aディレクトリではローカルは指定なし
- Bディレクトリではローカルをv8.11.3に指定

Aディレクトリの結果  
```
$pwd
/Users/<ユーザー>/testA
$ ls .node-version
ls: .node-version: No such file or directory
p$ node -v
v8.11.3
```  

Bディレクトリの結果  
```
$pwd
/Users/<ユーザー>/testB
$ ls .node-version
.node-version
$ cat .node-version
v8.11.2
$ node -v
v8.11.2
```  

無事にディレクトリを移動しただけで、  
node.jsのバージョンが自動的に変更されました。  

便利！  

\- 参考にさせていただきました -  
・[nodebrewからndenvにして、ディレクトリ/プロジェクト毎にnodeのバージョンを指定する方法](http://tnyk.jp/frontend/288/)  
・[ndenv を使用して複数のバージョンの Node.js を管理する方法と基本的な使い方](https://qiita.com/noraworld/items/462689e108c10102d51f)