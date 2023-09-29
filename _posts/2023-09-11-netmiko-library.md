---
layout: post
title: netmikoライブラリに関して
subtitle: netmiko library
gh-repo: junenu
gh-badge: [follow]
tags: [python,netmiko]
comments: true
---
## はじめに

Pythonの`netmiko`ライブラリを使用することで、ネットワーク機器にSSHで接続し、コマンドを実行したり、設定を変更したりすることができます。この記事では、`netmiko`ライブラリを使って、ネットワーク機器に接続し、コマンドを実行し、設定を変更する方法を解説します。

## 機器
- コマンドを実行する機器：MacOS（Pythonが動けばなんでもOKです）
- 対象のNW機器：vMX（Juniperさんが提供しているvLABs上の機器を利用）

## netmikoとは

`netmiko`は、複数のネットワークデバイスのベンダーに対応した、Pythonのライブラリです。`netmiko`を使用することで、SSHを使ってネットワークデバイスに接続し、コマンドの実行や設定の変更を行うことができます。

## Pythonスクリプト

以下は、`netmiko`を使ってJuniperのネットワークデバイスに接続し、ホスト名を変更するPythonスクリプトの例です。

```python
import netmiko
import subprocess

def send_show_command(device, commands):
    result = {}
    try:
        with netmiko.ConnectHandler(**device) as con:
            con.enable()
            for command in commands:
                output = con.send_command(command)
                result[command] = output
        return result
    except (netmiko.NetmikoTimeoutException, netmiko.NetmikoAuthenticationException) as e:
        print(e)

def send_config_set(device, commands):
    result = {}
    commands += ["commit"]
    try:
        with netmiko.ConnectHandler(**device) as con:
            con.enable()
            for command in commands:
                output = con.send_config_set(command)
                result[" ".join(command)] = output
        return result
    except (netmiko.NetmikoTimeoutException, netmiko.NetmikoAuthenticationException) as e:
        print(e)

if __name__ == "__main__":
    device = {
        'device_type': 'juniper',
        'host': '172.16.0.1', #自分の環境にあわせて指定すること
        'username': 'admin', #自分の環境にあわせて指定すること
        'password': 'admin', #自分の環境にあわせて指定すること
        'port': 22, #自分の環境にあわせて指定すること
    }

    before_result = send_show_command(device, ["show configuration | display set"])

    with open("conf/vMX_before.txt", "w") as f:
        f.write(before_result["show configuration | display set"])
    
    commands = ["set system host-name vMX"]
    result_conf = send_config_set(device, commands)

    after_result = send_show_command(device, ["show configuration | display set"])

    with open("conf/vMX_after.txt", "w") as f:
        f.write(after_result["show configuration | display set"])

    subprocess.run(["diff","-u", "conf/vMX_before.txt", "conf/vMX_after.txt"])
```

このスクリプトは、以下の手順で動作します。

1. `send_show_command`関数を定義。デバイス情報とコマンドを受け取って、コマンド入力した結果を返す
2. `send_config_set`関数を定義。デバイス情報とコマンドを受け取って、コマンド実行した結果を返す
3. デバイスの情報（ホスト名、ユーザー名、パスワード、ポート番号(※通常は22)）を設定します。
4. `send_show_command`関数を使って、デバイスの現在の設定を取得し、`conf`ディレクトリの`vMX_before.txt`に保存します。
5. `send_config_set`関数を使って、デバイスのホスト名を変更します。
6. 再度、`send_show_command`関数を使って、デバイスの設定を取得し、`conf`ディレクトリの`vMX_after.txt`に保存します。
7. `subprocess.run`関数を使って、`vMX_before.txt`と`vMX_after.txt`の差分を表示します。（終わり）

## 実行結果

スクリプトを実行すると、以下のような出力が得られます。host-nameの行だけ変更されていることが`diff`コマンドからわかります。

```
❯ python3.10 junos_conf.py
--- conf/vMX_before.txt 2023-09-11 15:29:06
+++ conf/vMX_after.txt  2023-09-11 15:29:17
@@ -1,5 +1,5 @@
 set version 21.1R3.11
-set system host-name vMX-addr-0
+set system host-name vMX
 set system root-authentication encrypted-password "$6$GKTs4Nse$lvSTPRqwuF/2WUea879/1lwrlJ1T3Wv3ccZIZ0L.2uao7gR3ywFRKvqfqGeN1Kpbmsvn2.vZ4OAZsa5hWz4al1"
 set system scripts language python3
 set system login user jcluser uid 2000
```

## 最後に
今回は`netmiko`ライブラリを利用してネットワーク機器の情報取得を設定変更を実施しました。シンプルな機能が備わっているためカスタマイズ性が高いライブラリなのかなーという印象でした。学習の一環としてテストコードとして例示したものよりかはさらに機能を加えて、より実用的なネットワーク自動化ツールを作成してみようかなと思っています。