# 体育館の予約を自動化
昨今の江東区バドミントン会場の混雑状況を受けて，より確実に予約が取れるように，江東区の体育館予約システムでの基本操作(ログイン・イベント一覧取得・予約・予約履歴の確認・キャンセル・複数人での予約)をpythonを使って(かなり雑に)CLI化してみました．

このコードではUIを経由せずに直接サーバに対してリクエストに送信するという実装になっています．

元々配布することを想定して作っていなかったので，必要最低限の処理しか書いておらず，必要に応じて各自で改造して使って貰えたらと思います．

alias作ってコマンド化してcrontabに指定の時間に実行するようにしたら画面を一々クリックしなくても予約ができるので速さで勝てると思います．ちなみに自分は念のために14:00になった瞬間に手動でコマンドを打っています．

バドミントンするだけなのにここまでするのかって自分でも思いますが，毎回予約が取れなくて結局今週もなしってなったら本当に悲しいので，みんなと楽しくバドができるように頑張ってみました．

## 使い方
pythonの実行環境を構築した状態でmain関数が書かれてあるスクリプトを引数指定で実行するという使い方になります．以下，ログイン，イベント一覧の取得，予約，予約履歴の確認およびキャンセルのやり方をそれぞれ説明します．
簡単のため，pythonで構築したpython環境を指します．

### ログイン
ログインするための関数をlogin.pyモジュール(pythonファイルをモジュールと呼びます)にまとめています．必要最低限にしか書きたくなかったのでユーザー名とパスワードはこのモジュール内の変数としてベタ書きして関数内で参照するという実装になっています．
ご自分のユーザー名とパスワードを変数に設定してください．

### イベント一覧の取得
python main.py showでその月のイベント情報を確認できます． 予約の際に必要なイベントIDの情報をこのコマンドの出力で確認することが多いです．予約可能な(つまりUIで青になっている)イベントに<-予約可能の矢印がついています．
江東区の予約システムの画面はリクエストした際にjavascriptなどによってページソースが動的に組まれているため，seleniumでスクレイピングして取得するしかありませんでした．エラーが出た場合はすみませんがpythonのseleniumがうまく動作していない可能性があるのでコードを修正したりで対応してください．また，seleniumでchromeを操作するのでchromeのインストールは必須です．

e.g.　出力例

<img width="633" alt="image" src="https://github.com/zhangchunpu/badminton_auto_reserve/assets/65750259/eb307dfb-7693-4d51-a9df-b0afb5ca6f55">

### 予約
python main.py reserve イベント番号　で予約します．
python main.py showで確認したイベント番号を指定して予約すると，ログインして予約してくれます．

### 予約履歴の確認
python main.py historyで予約履歴を確認します．使う場面としてキャンセルする際に予約IDを指定する必要があるのでこのコマンドの出力で確認する感じです．

e.g. 出力例

<img width="1339" alt="image" src="https://github.com/zhangchunpu/badminton_auto_reserve/assets/65750259/a09c3e53-b4b9-4e11-9730-98eef6607777">


### キャンセル
python main.py cancel 予約ID　でキャンセルします．
予約IDはpython main.py historyで確認できます．　イベントIDと異なるのでご注意ください．

### setup
pythonの構築および必要なライブラリのインストールが必要です．自分はpython 3.9.0を使いました．
pythonを普段からお使いになっている方が多いと思うので環境構築の方法はここでは説明しませんが，pyenvでの構築をおすすめします．必要なライブラリもpip  `install -r requirements.txt` でインストールできます．
pythonの環境構築方法: https://zenn.dev/kenghaya/articles/9f07914156fab5
