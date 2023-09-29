---
layout: post
title: napalmライブラリを使ってNW機器の情報を取得する
subtitle: napalm library
gh-repo: junenu
gh-badge: [follow]
tags: [python,napalm]
comments: true
---
# napalmを使用してCisco IOS-XRデバイスから情報を取得する方法

## napalmとは

napalm (Network Automation and Programmability Abstraction Layer with Multivendor support) は、異なるネットワークデバイスベンダー間で共通のAPIを提供するPythonライブラリです。この記事では、`napalm` を使用して Cisco IOS-XR デバイスに接続し、基本的な情報を取得する方法を説明します。

[https://napalm.readthedocs.io/en/latest/:embed:cite]

## 動作環境

### スクリプト実行側:
- **OS**: macOS
- **Pythonバージョン**: 3.10
- **実行環境**: Pythonの仮想環境 (`venv`) を使用して実行
- **事前準備**:pip3.10でnapalmをinstall 
     - `pip3.10 install napalm`

### 情報取得対象:
- **デバイス**: CiscoのiOS-XRを実行するデバイス
- **アクセス**: Cisco DevNet Sandbox を使用（注: 事前の登録が必要）

DevNet Sandboxは、Ciscoの最新の製品と技術を試すための無料の仮想環境を提供するプラットフォームです。このスクリプトでは、DevNet Sandbox上のiOS-XRデバイスから情報を取得します。DevNet Sandboxを利用するためには、Cisco DevNetのウェブサイトでの事前登録が必要です。



[https://developer.cisco.com/site/sandbox/:embed:cite]



## スクリプトの内容

まず、以下のスクリプト `isorx_show.py` をご紹介します：

```python
from napalm import get_network_driver

# ネットワークデバイスのドライバを取得
# 今回はiosxrに対して情報取得をするので`iosxr`とする。
driver = get_network_driver('iosxr')

# デバイスへの接続情報を設定
# (usernameやpassword cisco devnet sandboxにユーザー登録して得た情報を入力してください)
device = driver(
    hostname='sandbox-iosxr-1.cisco.com',
    username='admin',
    password='admin',
    optional_args={'port': 22}
)

# デバイスに接続
device.open()

# デバイス情報を取得
facts = device.get_facts()
print(type(facts))
print(facts)

# インターフェースの詳細情報を取得
interfaces = device.get_interfaces()
print(type(interfaces))
print(interfaces)

# 接続を終了
device.close()
```

## スクリプトの実行結果

スクリプトを実行すると、以下のような出力が得られます：

[出力内容]

```bash
$ python3.10 isorx_show.py
<class 'dict'>
{'fqdn': 'ios',
 'hostname': 'ios',
 'interface_list': ['GigabitEthernet0/0/0/0',
                    'GigabitEthernet0/0/0/1',
                    'GigabitEthernet0/0/0/2',
                    'GigabitEthernet0/0/0/3',
                    'GigabitEthernet0/0/0/4',
                    'GigabitEthernet0/0/0/5',
                    'GigabitEthernet0/0/0/6',
                    'Loopback100',
                    'Loopback555',
                    'MgmtEth0/RP0/CPU0/0',
                    'Null0'],
 'model': 'R-IOSXRV9000-CC',
 'os_version': '7.3.2',
 'serial_number': 'B550ED1D0D9',
 'uptime': 204207.0,
 'vendor': 'Cisco'}
<class 'dict'>
{'GigabitEthernet0/0/0/0': {'description': '',
                            'is_enabled': True,
                            'is_up': False,
                            'last_flapped': -1.0,
                            'mac_address': '00:50:56:BF:BB:17',
                            'mtu': 1514,
                            'speed': 1000.0},
 'GigabitEthernet0/0/0/1': {'description': 'test',
                            'is_enabled': False,
                            'is_up': False,
                            'last_flapped': -1.0,
                            'mac_address': '00:50:56:BF:13:37',
                            'mtu': 1514,
                            'speed': 1000.0},
 'GigabitEthernet0/0/0/2': {'description': 'test',
                            'is_enabled': False,
                            'is_up': False,
                            'last_flapped': -1.0,
                            'mac_address': '00:50:56:BF:81:81',
                            'mtu': 1514,
                            'speed': 1000.0},
 'GigabitEthernet0/0/0/3': {'description': 'test',
                            'is_enabled': False,
                            'is_up': False,
                            'last_flapped': -1.0,
                            'mac_address': '00:50:56:BF:AF:C9',
                            'mtu': 1514,
                            'speed': 1000.0},
 'GigabitEthernet0/0/0/4': {'description': '',
                            'is_enabled': False,
                            'is_up': False,
                            'last_flapped': -1.0,
                            'mac_address': '00:50:56:BF:BE:DB',
                            'mtu': 1514,
                            'speed': 1000.0},
 'GigabitEthernet0/0/0/5': {'description': '',
                            'is_enabled': False,
                            'is_up': False,
                            'last_flapped': -1.0,
                            'mac_address': '00:50:56:BF:3E:E8',
                            'mtu': 1514,
                            'speed': 1000.0},
 'GigabitEthernet0/0/0/6': {'description': '',
                            'is_enabled': False,
                            'is_up': False,
                            'last_flapped': -1.0,
                            'mac_address': '00:50:56:BF:EE:29',
                            'mtu': 1514,
                            'speed': 1000.0},
 'Loopback100': {'description': '***TEST LOOPBACK****',
                 'is_enabled': True,
                 'is_up': True,
                 'last_flapped': -1.0,
                 'mac_address': '',
                 'mtu': 1500,
                 'speed': 0.0},
 'Loopback555': {'description': 'PRUEBA_KV',
                 'is_enabled': True,
                 'is_up': True,
                 'last_flapped': -1.0,
                 'mac_address': '',
                 'mtu': 1500,
                 'speed': 0.0},
 'MgmtEth0/RP0/CPU0/0': {'description': '',
                         'is_enabled': True,
                         'is_up': True,
                         'last_flapped': -1.0,
                         'mac_address': '00:50:56:BF:D3:07',
                         'mtu': 1514,
                         'speed': 1000.0},
 'Null0': {'description': '',
           'is_enabled': True,
           'is_up': True,
           'last_flapped': -1.0,
           'mac_address': '',
           'mtu': 1500,
           'speed': 0.0}}
```

## 解説

このスクリプトは、Cisco DevNetのsandbox環境に存在するIOS-XRデバイスに接続し、基本的なデバイス情報 (`get_facts`) とインターフェース情報 (`get_interfaces`) を取得します。

`get_facts` メソッドを使用すると、デバイスのFQDN、ホスト名、インターフェースのリスト、モデル名、OSバージョン、シリアル番号、稼働時間、ベンダー名などの基本情報を取得できます。

一方、`get_interfaces` メソッドを使用すると、各インターフェースの詳細情報、例えば、インターフェースの説明、インターフェースの有効/無効状態、インターフェースのUp/Down状態、MACアドレス、MTU、速度などの情報を取得できます。

## まとめ

`napalm` ライブラリを使用すると、簡単に様々なネットワークデバイスベンダーから情報を取得したり、設定を変更したりすることができます。pythonでは辞書型でresponceが返されるので`json`モジュールでjson形式に変換することもできます。
