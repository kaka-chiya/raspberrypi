# raspberrypi

## env
- Raspberry Pi 3b + (以下、raspi)
- Aquos TV
- Macbook Pro 2019(2.3 GHz 8Core-Intel Core i9/ 32 GB 2667 MHz DDR4/ Intel UHD Graphics 630 1536 MB)
- Mouse (Logicool/ Wireless)
- Keyboard (Logicool/ Wireless)

## setup[JA]
1. raspiにSSH接続する準備
- misroSDをmacに挿す
- boot/(=microSDのroot directory)階層にsshという名前の空ファイルを作ってSSH接続を有効にする
- macのTerminalを立ち上げ、下記を実行する
- `$ touch /Volumes/boot/ssh`

2. raspiをeifi接続させる
- boot/階層に以下のファイルを作成する
- ファイル名は `wpa_supplicant.conf`
```sh
country=JP
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="wifiのSSID"
    psk="wifiのPW"
}
```

3. ssh接続
- misroSDをraspiに挿す
- raspiの電源をONし、OSが起動してホーム画面に遷移するまで待機
- 以下コマンドをmacのTerminalで実行
- `$ ssh pi@raspberrypi.local`
  - PWは好きなものを設定
- Terminalのユーザー名が`pi@raspberrypi:~ $`に変わったことを確認＝macからraspiへssh接続できている状態

4. ipアドレスの調査
- 3.まででssh接続できているが、CUI操作だけじゃなくてGUI操作もしたい。つまり画面共有。
- vnc viewer使ったことある人ならなんとなくわかると思う。
- sshを一度抜ける。`$ exit`
- <b>macのTerminal</b>で`$ arp -a`
  - `? (IPアドレス) at MACアドレス on ~~~~~ [ethernet]`が表示されるので(IPアドレス)をメモする。
  - raspiのIPは`b8:27:eb`から始まる。
- もう一度macでssh接続する
  - `$ ssh pi@raspberrypi.local`
- raspiにvncサーバーをインストール
  - `pi@raspberrypi:~ $ sudo apt-get install tightvncserver`
- raspiでvncサーバー起動
  - `pi@raspberrypi:~ $ sudo tightvncserver`
- パスワードが聞かれるので好きなものを。
  - `New 'X' desktop is raspberrypi:1`と表示される。
  - raspberrypi:1の数字(例だと1)を覚えておく

5. vnc接続
- macのFinderから移動＞サーバーへ移動
- サーバーアドレス入力画面で以下を入力して接続
  - `vnc://IPアドレス:ポート番号`
  - IPアドレスは4.でメモしたものを入力。
  - ポート番号は、590+raspberrypi:の数字。
    - ポート番号は5900番台。
    - 4.の例だと、`5901`


以上