## Chabot試した
### ~ChatWork小ネタ~

---

## 事情
+ ExplorienceでG2ヘルプ
+ G1エンジニアは[Slack](https://slack.com/)でコミュニケーション
 - Slack上でGitlabのpush通知を受け取ってMergeRequestの処理を行ったり
+ G2メンバにも通知したい。。。
 - 通知ないと、いちいち席立って声掛けする
 - 通知があってもスルーすることはあるけど、、、まぁそれはさ、、、
 - Slackにゲストで参加させるのはChannelにつき1人である


### とりまChatWorkに通知するか

--

### ってことで,Chabotを試す
+ Chabotってなにさ
 - ggrks
 - ChatWork と Webhook を提供しているサービスとを連携させるアプリケーションです。
 - Gitlab - Chabot - ChatWork のような連携

--

### やりたいことの流れ
1. エンジニア   が GitlabにMR
2. Gitlab       が Webhook使って, Chatbotアプリ(Herokuに載せる)に通知
3. Chabotアプリ が ChatWork APIを叩いて、ChatWorkにPost
4. エンジニア   は 通知を見てレビュ 

---

### [デモ](http://dev1.hyakuren.org:18080/training2013/mvp2)

---

### 導入準備
* Heroku
 - Chabot(Gitlab-ChatWorkを実現する)アプリを動かしておく
* ChatWork
 - Chabotを使うにあたり、ChatWorkのAPI叩く必要あり 
 - APIを使用するには、申請が必要
 - 三口さんにネゴる
* Gitlab
 - Webhookの設定, どこに対して通知するのか
 - 設定にはmaster権限が必要
 - 理解を求めて、@yshiga(百錬Gitlabの構築者), @kimura, @iizuka(お二人も管理者相当の権限はお持ち)さんあたりにネゴる

--

## 各所のネゴが一番手間ｗ
### (いやまぁ、言うだけなんだけど色々あるやん?)

--

### Heroku
アプリをpushするだけ、割愛

--

### ChatWork
* API申請が通ったらtokenが発行されるので控える
* 通知したいroom_idを控える(#!rid12345678の12345678部)

--

### Chabotアプリ作成

+ [基本的にREADME通りつくる](https://github.com/zaru/chabot/blob/master/README.md)

```html
~/work/a$ chabot create test
  copying files.
  completed!
   > /Users/motosato/work/a/test
~/work/a$ ll
total 0
drwxr-xr-x  3 motosato  staff   102B  4  8 13:50 ./
drwxr-xr-x  8 motosato  staff   272B  4  8 13:50 ../
drwxr-xr-x  8 motosato  staff   272B  4  8 13:50 test/
~/work/a$ cd test/
~/work/a/test$ ll
total 24
drwxr-xr-x  8 motosato  staff   272B  4  8 13:50 ./
drwxr-xr-x  3 motosato  staff   102B  4  8 13:50 ../
-rw-r--r--  1 motosato  staff   290B  4  8 13:50 app.js
drwxr-xr-x  3 motosato  staff   102B  4  8 13:50 bots/
-rw-r--r--  1 motosato  staff   189B  4  8 13:50 config.json
drwxr-xr-x  3 motosato  staff   102B  4  8 13:50 node_modules/
-rw-r--r--  1 motosato  staff   229B  4  8 13:50 package.json
drwxr-xr-x  3 motosato  staff   102B  4  8 13:50 templates/
~
```

--

#### config.json書き換え

```html
{
    "port": 5000,
    "bots": {
+        "gitlab": {
+            "hostname": "herokuに載せてるappのurl",
+            "token": "CWのtoken",
+            "route": "/gitlab/hooks/:roomid"
        }
    }
}
```

--

#### アプリを動かす

+ `Procfile` つくって、`nodeで走らせる`

```html
web: node app.js
```

--

### Gitlabの設定

+ [プロジェクトの設定画面](http://dev1.hyakuren.org:18080/training2013/mvp2/hooks)でWebhookを登録
+ herokuアプリのurl/gitlab/hooks/通知したいroom_id
 - TriggerはMerge Requests events

---

### 以上でとりまChatWork通知はできる

--

#### 問題点(というか時間ない)
+ assineeが返ってこないので誰にレビュ依頼してるかわからん
+ stateが `opened` しか返ってこない(mergedが返ってこない)

---

糸冬
