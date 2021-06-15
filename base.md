---
marp: true
---
# OAuth 2.0 勉強会

山内 雅浩
[@algas](https://github.com/algas)

---

# 発表概要

1. OAuth2.0とは何か
1. RFC6749を読む
1. OAuth2.0実装例の紹介

---

# OAuth2.0とは何か

---

# OAuth2.0とは何か
## 認証と認可

- 認証: アクセスしたのが誰であるかを確認すること
- 認可: リソースへのアクセス権限を与えること
    - OAuth はこっち
    - 認可自体は認証と関係ない(印鑑や切符は持ってれば誰でも使える)

---

# OAuth2.0とは何か
## 認可の具体例

たとえば、クラウド上の写真データを印刷サービスを使ってプリントしたいとする
でも、写真データにアクセスするパスワードを印刷サービスには教えたくない
パスワードを教えずに印刷サービスが写真データにアクセスさせるために「認可」を使う

---

# OAuth2.0とは何か
## OAuth2.0の概要

- OAuth 2.0 は HTTP サービスへの限定的なアクセスを可能にする認可フレームワークである
- 制限されたリソースにアクセスするためのトークンを取得する方法とそのトークンの使い方を定義したものである

---

# OAuth2.0とは何か
## OAuth 2.0 は何では「ない」か

- OAuth は HTTP プロトコルを使わずに定義されることではない
- OAuth は認証のプロトコルではない
- OAuth ではあるユーザから別のユーザへの権限委譲の中身を定義していない
- OAuth は認可プロセスの仕組みを理解していない
- OAuth は Token のフォーマットを定義していない
- OAuth は暗号に関する方法について定義していない
- OAuth 2.0 は単一のプロトコルではない

(書籍 「OAuth徹底入門(1章5節)」から引用)

--- 

# RFC6749を読む
## RFC6749について

https://datatracker.ietf.org/doc/html/rfc6749

- "The OAuth 2.0 Authorization Framework"
- Internet Engineering Task Force, Requests for Comment 6749
- インターネット技術特別技術委員会による技術文章

--- 

# RFC6749を読む
## Abstract (Page 1)

> OAuth 2.0 は, サードパーティーアプリケーションによるHTTPサービスへの限定的なアクセスを可能にする認可フレームワークである. サードパーティーアプリケーションによるアクセス権の取得には, リソースオーナーとHTTPサービスの間で同意のためのインタラクションを伴う場合もあるが, サードパーティーアプリケーション自身が自らの権限においてアクセスを許可する場合もある. 本仕様書はRFC 5849に記載されているOAuth 1.0 プロトコルを廃止し, その代替となるものである.

引用: https://openid-foundation-japan.github.io/rfc6749.ja.html

---

# RFC6749を読む
## Roles (Page 6)

1. Resource Owner: 認可を渡す役割
ユーザのWebブラウザ
2. Resource Server: 保護されたリソースをホストするサーバ
写真データを持つサーバ
3. Client: 認可をもらう役割
印刷サービス
4. Authorization Server: 認可のためのトークンを発行するサーバ
OAuth のサーバ

---

# RFC6749を読む
## Protocol Flow (Page 7)

3行でわかるプロトコルの流れ

1. Client -> Resource Owner: リソースにアクセスしたいので Grant ください
2. Client -> Authorization Server: Grant あるので Access Token ください
3. Client -> Resource Server: Access Token あるのでリソースください

(図)

---

# RFC6749を読む
## Authorization Grant (Page 8)

4種類のGrant(権限の渡し方)

1. Authorization Code
2. Implicit
3. Resource Owner Password Credentials
4. Client Credentials

---

# RFC6749を読む
## Grant 1: Authorization Code (Page 8,24)

(図)

---

# RFC6749を読む
## Grant 2: Implicit (Page 8,31)

(図)

---

# RFC6749を読む
## Grant 3: Resource Owner Password Credentials (Page 9,37)

(図)

---

# RFC6749を読む
## Grant 4: Client Credentials (Page 9,40)

(図)

---

# RFC6749を読む
## Access Token (Page 10,43)

制限されたリソースにアクセスするための鍵のようなもの
トークンのフォーマットなどは定義されてない
(あとで書く)

---

# RFC6749を読む
## Refresh Token (Page 10,47)

3行でわかる Refresh Token

1. Client -> Resource Server: Access Token あるのでリソースください(Token古いからダメ)
2. Client -> Authorization Server: Refresh Token あるから新しい Access Token ください(新しい Access Token と Refresh Token もらう)
3. Client -> Resource Server: Access Token あるのでリソースください(リソースをもらう)

ポイントは Resource Owner が出てこないこと

---

# RFC6749を読む
## Protocol Entpoints (Page 18)

2つの認可サーバエンドポイント

- Authorization Entpoint (`/authorize`): Client が Resource Owner から認可を得るのに使う
- Token Endpoint (`/token`): Client が Access Token と Grant を交換するために使う
