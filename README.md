# 訪問者を識別するIoTインターホン powered by Einstein Vision

画像認識AI"Einstein Vision"の実装例のサンプルです。 呼び鈴はラズベリーパイで、受話器はSalesforceインターフェース上に、アプリケーションとして構築します。

※Einstein Vision 公式ドキュメント ⇒ <https://metamind.readme.io/v2/docs>

![アーキテクチャ全体](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img001.png)

## 準備するもの
### - ハードウェア(呼び鈴)
* Raspberry Pi 3 Model B
* Raspberry Pi カメラモジュール
* Raspberry Pi Sense Hat

### - アカウント（処理サーバ / 受話器アプリケーション）
* Herokuアカウント ※サインアップ ⇒ <https://signup.heroku.com/>
* Salesforce Developer Edition組織のアカウント　※サインアップ ⇒ <http://developer.salesforce.com/signup>

## セットアップ
### - Salesforceの設定
受話器アプリケーションのサンプルパッケージをインストールし、有効化するための設定をします。  
  
1.受話器アプリケーションサンプルパッケージをインストール
<https://login.salesforce.com/------working now------->


2. ユーティリティバー上にLightning Component""を配置する。

3. ....

### - Herokuの設定
呼び鈴と受話器アプリケーションを連携させるためのサーバアプリケーションをインストールします。  
  
1. [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/misu007/iot-intercom-with-einstein-vision-example-heroku/tree/master)をクリック。

2. 環境変数を設定。


### - Raspberry Piの設定
呼び鈴として、Raspberry Piと各種部品の組み立て、アプリケーションをインストールします。  
  
1. ハードの組み立て
* Raspberry Pi Model B
* Raspberry Pi カメラモジュール
* Raspberry Pi Sense Hat
を組み立てる。

2. 適当なディレクトリを作成・移動（ここでは、~/nodeとする）
`cd ~/`  
`mkdir node`  
`cd node`  

3. アプリケーションのインストール

`git clone https://----------working now--------------`  
`cd --app name--`  
`npm install`  

4. domeinURLを2.で作成したherokuアプリのURLに書き換える。
`emacs index.js`  


## 動作確認
1. Salesforceにログインし、画面を開いておく。

2. Raspberry Piのアプリケーションを起動する。
`npm start`

3. Raspberry Pi Sense Hat上のジョイスティックをクリックする。

4. Salesforce


## 関連リポジトリ
<https://github.com/misu007/iot-intercom-with-einstein-vision-example-heroku>  
<https://github.com/misu007/iot-intercom-with-einstein-vision-example-raspberrypi>  

## 免責事項
このサンプルコードは、あくまで機能利用の1例を示すためのものであり、コードの書き方や特定ライブラリの利用を推奨したり、機能提供を保証するものではありません。


