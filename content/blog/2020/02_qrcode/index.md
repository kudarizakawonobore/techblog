---
title: "Raspberry Pi で QR コードリーダーからプログラムを動かす"
date: 2020-02-20T23:09:00+09:00
tags: [raspberrypi, python, qrcode]
---

## 概要
市販のQRコードリーダーを起動契機にして、何かしらの処理を行うデバイスを作りたいときの実装メモ。

## 環境
Raspberry Pi 3
OS: Raspbian
Python: 3.6.5

## 知っておきたいこと
USB タイプの QR コードリーダーや、バーコードリーダーは、キーボードと同様の入力デバイスとして扱うことができる。

QRコードリーダーを RaspberryPi に差した状態で、QR コードを Read する（＝デコードする）と、市販の QRコードリーダーがテキスト入力として PC にイベントを送ってくる。
なので、単に値を解読したいだけなら、テキストエディタを開いた状態で、QRコードを読み込んであげるだけでよい。

自作のアプリから、デコードした値を処理したいなら、キーボードのイベントをListen し、Enterキーが押されたのを起動契機に処理を実行するプログラムを書いてあげればよい。

## 候補
Python でキーボードの入力を取得するためのライブラリはいくつかあるが、今回は `keyboard` を採用した。

下記は、不採用になったものを含めたキーボードの入力を取得するためのライブラリ。

### pynput(不採用)
キーボードやマウスの入力を Listen できるライブラリ。
`keyboard` 同様、文字列として入力を受け付けられるが、 SSH 経由でバックグラウンド実行させることができなさそうなので、不採用。

### envdev(不採用)
pynput と異なり、SSH 経由でバックグラウンド実行させられる他、デバイスを指定することができる。
一方で、発生したイベントがコード文字列(KEY_A や KEY_B など）になってしまうためコードを文字列に戻してやる処理が必要になる。

### keyboard(採用)
バックグラウンドで実行でき、文字列をわりと簡単に処理できるので、採用。

## インストール
pip で keyboard をインストールする

```shell
 pip install keyboard
```

## 実装

```python
import keyboard
import time

qr = ""

def key_press(key):
    global qr
    if key.name == "enter":
        print(qr)
    elif 'shift' in key.name:
        qr += key.name.lstrip('shift')
        return
    else:
        qr += key.name
        return
    qr = ""

keyboard.on_press(key_press)
while True:
    time.sleep(1)
```

## 追記(2020/02/25)
keyboard では、大文字や shift キーを押した際の文字（？など）を `shiftX` のように認識してしまうことが判明。
key.name が shift を含んでいたら、shiftをトリミングするように修正して、完了。

## 参考
https://qiita.com/teraken_/items/0e8c5b31567f966773b6

https://tutorialmore.com/questions-559657.htm

https://qiita.com/teraken_/items/0e8c5b31567f966773b6

https://qiita.com/takaken/items/9c5a5d7f3a9b39f5bc04
