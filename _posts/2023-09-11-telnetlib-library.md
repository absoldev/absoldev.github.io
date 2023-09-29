---
layout: post
title: telnetlibライブラリに関して
subtitle: telnetlib library
gh-repo: junenu
gh-badge: [follow]
tags: [python,telnetlib]
comments: true
---
## はじめに

`telnetlib`は、Pythonの標準ライブラリの一つで、Telnetセッションを制御するための機能を提供します。この記事では、Pythonの`telnetlib`モジュールを使用して、ネットワークデバイスにTelnetで接続し、コマンドを実行する方法について説明します。

## telnetlibとは

`telnetlib`は、Pythonの標準ライブラリの一つで、Telnetクライアントを作成するためのクラスを提供します。このライブラリを使用することで、PythonスクリプトからTelnetサーバーに接続し、コマンドを実行したり、サーバーからのレスポンスを取得したりすることができます。

[https://docs.python.org/ja/3/library/telnetlib.html:embed:cite]


## 実装

以下は、`telnetlib`を使用したPythonスクリプトの例です。

```python
import telnetlib
from getpass import getpass

# 接続先の情報
HOST = "192.168.11.211"
USERNAME = input("Username: ")
PASSWORD = getpass()

# Telnetセッションの開始
tn = telnetlib.Telnet(HOST)

# ログイン
tn.read_until(b"Username: ")
tn.write(USERNAME.encode('ascii') + b"\n")
if PASSWORD:
    tn.read_until(b"Password: ")
    tn.write(PASSWORD.encode('ascii') + b"\n")

# コマンドの実行
tn.write(b"show version\n")
tn.write(b"exit\n")

# 結果の表示
print(tn.read_all().decode('ascii'))

# セッションの終了
tn.close()
```

このスクリプトは以下の手順で動作します。

1. ユーザーからホスト、ユーザー名、およびパスワードの情報を取得します。ユーザー名の入力には``input``を利用し、パスワードの入力には``getpass``を利用しています。
2. Telnetセッションを開始し、取得した情報を使ってログインします。
3. ログインが成功したら、`show version`コマンドを実行し、その後`exit`コマンドを実行してセッションを終了します。
4. サーバーからの全てのレスポンスを受信し、コンソールに表示します。
5. Telnetセッションを閉じます。

## 実行結果

スクリプトを実行すると、以下のような出力が得られます。対象のNW機器のOSにはAristaさんのvEOSを利用しています。

```
❯ python3.10 test.py

Last login: Mon Sep 11 02:37:57 from 192.168.10.157
show version
exit
eos01>show version
 vEOS
Hardware version:      
Serial number:         
Hardware MAC address:  5000.0072.8b31
System MAC address:    5000.0072.8b31

Software image version: 4.24.11M
Architecture:           i686
Internal build version: 4.24.11M-28848826.42411M
Internal build ID:      27bc0c4a-415b-49ca-8ad9-eb1a4323df22

Uptime:                 0 weeks, 0 days, 2 hours and 29 minutes
Total memory:           2014548 kB
Free memory:            1316888 kB

eos01>exit
```

## まとめ

Pythonの`telnetlib`モジュールを使えば、Telnetを使ってネットワークデバイスに接続し、コマンドを実行することができます。ただし、セキュリティの観点から、実運用環境ではTelnetよりも安全なプロトコル（例：SSH）を使用するのが一般的かと思いますので、あくまで一つの手段に過ぎないかなと思っています。

## 注意事項（念のため）

Telnetはセキュリティが非常に低いプロトコルです。ユーザー名、パスワード、コマンド、レスポンスなど、全ての通信が平文で送信されます。