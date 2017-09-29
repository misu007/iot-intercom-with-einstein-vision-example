# 訪問者を識別するIoTインターホン powered by Einstein Vision

画像認識AI"Einstein Vision"の実装例サンプルです。 呼び鈴はラズベリーパイで、受話器はSalesforceインターフェース上に、アプリケーションとして構築します。

※Einstein Vision 公式ドキュメント ⇒ <https://metamind.readme.io/v2/docs>

![アーキテクチャ全体](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img001.png)

## 準備するもの
### - ハードウェア(呼び鈴)
* Raspberry Pi 3 Model B
* Raspberry Pi カメラ モジュール
* Raspberry Pi Sense Hat モジュール

### - アカウント（処理サーバ / 受話器アプリケーション）
* Herokuアカウント ※サインアップ ⇒ <https://signup.heroku.com/>
* Salesforce Developer Edition組織のアカウント　※サインアップ ⇒ <http://developer.salesforce.com/signup>

## セットアップ
### (1) Salesforce接続アプリケーションの作成
Salesforce Developer Edition組織にログインし、外部からSalesforceへアクセスするためのSalesforce接続アプリケーションを作成します。    

1. 接続アプリケーションを作成します。
![新規接続アプリケーション](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img101.png)

2. 接続アプリケーション名・メールアドレスなどの必要事項入力し、OAuthを有効化し、保存します。
![OAuth有効化](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img102.png)

3. 作成した接続アプリケーションの"コンシューマ鍵"と"コンシューマの秘密"をメモし、さらに"Manage"をクリックします。
![Manageクリック](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img103.png)

4. OAuthポリシーを、"すべてのユーザは自己承認可能"、"IP制限の緩和"に変更して保存します。
![OAuthポリシー](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img104.png)


### (2) Heroku設定
呼び鈴と受話器アプリケーションを連携させるためのサーバアプリケーションをHeroku上にインストールします。  
  
1. [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/misu007/iot-intercom-with-einstein-vision-example-heroku/tree/master)をクリック。

2. アプリ名、環境変数などを設定します。
* **APP_CLIENT_ID** ⇒ **(1) Salesforce接続アプリケーション**の3.で取得した"コンシューマ鍵"
* **APP_CLIENT_SECRET** ⇒ **(1) Salesforce接続アプリケーション**の3.で取得した"コンシューマの秘密"
* **SALESFORCE_USERNAME** ⇒ Salesforce Developer Edition組織のユーザ名
* **SALESFORCE_PASSWORD** ⇒ Salesforce Developer Edition組織のパスワード

### (3) Salesforce設定
Salesforce Developer Edition組織に受話器アプリケーションのパッケージをインストールし、各種設定をし有効化します。  

1. ↓URLへアクセスし、受話器アプリケーションサンプルパッケージをインストールします。 
<https://login.salesforce.com/packaging/installPackage.apexp?p0=04t7F000001tyJQ>

2. カスタム設定で、**(2) Heroku設定**で決めたHerokuアプリ名を登録します。
![カスタム設定](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img301.png)
![カスタム設定](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img302.png)
![カスタム設定](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img303.png)

3. リモートサイトにherokuアプリのドメイン"https://[**(2) Heroku設定**で決めたHerokuアプリ名].herokuapp.com"を登録します。
![リモートサイト](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img311.png)
![リモートサイト](https://github.com/misu007/iot-intercom-with-einstein-vision-example/raw/master/img/img312.png)

### (4) Raspberry Pi設定
呼び鈴として、Raspberry Piと各種部品の組み立て、アプリケーションをインストールします。  
  
1. Raspberry Piを組み立て、公式にサポートされているOS"Raspbian"をインストールします。 ※手順は省略

2. 適当なディレクトリを作成・移動（ここでは、~/nodeとする）  
`cd ~/`  
`mkdir node`  
`cd node`  

3. アプリケーションのインストール  
`git clone https://github.com/misu007/iot-intercom-with-einstein-vision-example-raspberrypi.git`  
`cd iot-intercom-with-einstein-vision-example-raspberrypi`  
`npm install`  

4. **(2) Heroku設定**で作成したHerokuアプリにアクセスするように、一部ソースコードを書き換えます。  
`emacs index.js`  
等で**index.js**を開き、7行目のherokuDomainの値を  
`https://[**(2) Heroku設定**で決めたHerokuアプリ名].herokuapp.com`  
に変更し、保存します。

## 動作確認
1. Salesforce Developer Edition組織にログインし、Lightning ExperienceのUIで"インターホン受話器"アプリケーションを開きます。

2. Raspberry Piのアプリケーションを起動します。  
`cd ~/node/iot-intercom-with-einstein-vision-example-raspberrypi/`  
`npm start`  

3. Raspberry Pi Sense Hat上のジョイスティックをクリックします。


4. Salesforce Developer Edition組織の受話器アプリケーションが起動し、カメラ画像、画像認識結果が表示されれば成功です。　※通話機能は実装していません。

## 独自の画像識別機能を追加
デフォルトでは、Einstein Visionの標準モデルの一つである"GeneralImageClassifier"を利用する設定になっていますが、簡単な設定作業で、独自のカスタムモデルを使用するよう変更可能です。

1. Einstein Visionのカスタムモデルを作成します ※手順省略

2. 作成したHerokuアプリの環境変数"EINSTEIN_MODEL_ID"の値を、作成したカスタムモデルのIDに変更します。


## 関連リポジトリ
* Heroku用Node.jsアプリ  
<https://github.com/misu007/iot-intercom-with-einstein-vision-example-heroku>  
* Raspberry Pi用Node.jsアプリ  
<https://github.com/misu007/iot-intercom-with-einstein-vision-example-raspberrypi>  

## 免責事項
すべてのコンテンツは、あくまで機能利用の一例を示すためのものであり、コードの書き方や特定ライブラリの利用を推奨したり、機能提供を保証するものではありません。


