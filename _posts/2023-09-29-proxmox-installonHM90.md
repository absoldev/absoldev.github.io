---
layout: post
title: ProxmoxをHM90にインストールする
subtitle: proxmox install on HM90
gh-repo: junenu
gh-badge: [follow]
tags: [proxmox]
comments: true
---
# Proxmox install on HM90
## Install
### Download
Proxmox VE ISO Installerをインストールします。[こちら](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso)

### Install
インストールは、[こちら](https://pve.proxmox.com/wiki/Installation)を参考に

## Tips
1. HM90のBIOS設定で、VT-dを有効にする必要があります。
2. USBbootをする場合、BIOS設定で、USBのブート順位を上げる必要があります。
3. Proxmoxのインストール後、USBbootをする場合、BIOS設定で、USBのブート順位を下げる必要があります。
4. USBを挿入する箇所は、前面（電源ボタンがある方）で実施する必要があります。逆側では、USBbootができませんでした。（kernel panicエラーが出ます）
5. インストールの際に3%で20-30分ほど止まりますが、そのまま放置しておくと、インストールが完了します。