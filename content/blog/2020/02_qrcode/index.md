---
title: "Raspberry Pi で QR コードリーダーからプログラムを動かす"
date: 2020-02-20T23:09:00+09:00
tags: [raspberrypi, python, qrcode]
---

## 概要
市販のQRコードリーダーを使って、処理を行うデバイスを Raspberrypi で作るときの実装について書く。

## 知っておきたいこと
USB タイプの QR コードリーダーや、バーコードリーダーは、キーボードと同様の入力デバイスとして扱われる。

QR コードを Read する（＝デコードする）と、市販の QRコードリーダーが
テキスト文字列として PC に入力してくれる。

単に値を解読したいだけなら、テキストエディタを開いて読み込んであげるだけでよい。
自作のアプリを作って、解読した値を渡したいなら、キーボードのイベントをListenするプログラムを書いてあげればよい。

## 環境
Raspberry Pi 3
OS: Raspbian
Python: 3.6.5

## 候補
Python でキーボードの入力を取得するためのライブラリはいくつかあるが、今回は `keyboard` を採用した。

下記は、キーボードの入力を取得するためのライブラリ。

### pynput(不採用)
キーボードやマウスの入力を Listen できるライブラリ。
`keyboard` 同様、文字列として入力を受け付けられるが、バックグラウンドで実行させることができなさそうなので、不採用。

### envdev(不採用)
pynput と異なり、バックグラウンドで実行させられる他、デバイスを指定することができる。
一方で、発生したイベントがコード文字列(KEY_A や KEY_B など）になってしまうためコードを文字列に戻してやる処理が必要になる。

### keybard(採用)
バックグラウンドで実行でき、文字列をそのまま入力できるので、採用。

## インストール
pip で keyboard をインストールする

```shell
 pip install keybard
```

## 実装

```python
import keyboard
import time

qr = ""

def key_press(key):
    global qr
    if key.name != "enter":
        try:
          qr += key.name
          return
        except:
          return
    else:
        print(qr)
    qr = ""

keyboard.on_press(key_press)
while True:
    time.sleep(1)
```

## 参考
https://qiita.com/teraken_/items/0e8c5b31567f966773b6

https://tutorialmore.com/questions-559657.htm

https://qiita.com/teraken_/items/0e8c5b31567f966773b6

https://qiita.com/takaken/items/9c5a5d7f3a9b39f5bc04