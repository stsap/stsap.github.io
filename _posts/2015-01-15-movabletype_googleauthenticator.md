---
layout: post
title: GoogleAuthenticatorとかいう有能
---

最近自分のMicrosoftアカウントへの不正アクセス通知がやたらに飛んでくるので重い腰をあげて2要素認証を設定した。  
恥ずかしながらこの時に知ったんだけどGoogleAuthenticatorとかいう有能アンド有能なスマホアプリで2要素認証できるようになってるのね。  

で、思い立ったので仕事の一環としてMovable TypeでGoogleAuthenticatorを使って2要素認証するプラグインを書いてみた。

[mt-plugin-TwoFactorAuthGoogle](https://github.com/stsap/mt-plugin-TwoFactorAuthGoogle)  
意外と苦労した。  

## 苦労ポイント

### Auth::GoogleAuthenticatorが入らない

[Auth::GoogleAuthenticator](http://search.cpan.org/~corion/Auth-GoogleAuthenticator-0.02/lib/Auth/GoogleAuthenticator.pm)使うのが楽そうだったので入れた。  
そしたらMoose依存だったり色々で結構苦労した。

### 初期構想は無理だった

社内の誰かが「普通にログインさせた後に2要素認証用の追加画面出して云々」と言っていたのでそのまま実装しようとした。  
2要素認証用の画面出た時にはログイン済みなので、URLパラメータ書き換えたら普通に操作できちゃうじゃん。  
これをなんとかしようとしてごちゃごちゃやって苦労した。  
結局この実装をやめた。

### クエリパラメータが復活する問題

ログインに失敗して再度ログインフォームが表示された後、何度正しい認証情報でログインしようとしてもダメだった。  
なんでかなーと思ったら、認証失敗した時のワンタイムパスワードがログイン画面のform内にhiddenで付いてた。  

## インストール

* Auth::GoogleAuthenticator モジュールを入れる
* mt-plugin-TwoFactorAuthGoogle をリポジトリから取ってきて plugins/ に放り込む
* お手元のスマホに[GoogleAuthenticatorアプリ](https://www.google.co.jp/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0CCMQFjAB&url=https%3A%2F%2Fitunes.apple.com%2Fjp%2Fapp%2Fgoogle-authenticator%2Fid388497605%3Fmt%3D8&ei=FGG4VMD2I-exmwXpnoKgAQ&usg=AFQjCNH4_nazIxck2RZDONKAlbdV9P1f8A&sig2=KMcvohoO9r1XFf6Pz7BJtg)を入れる

## 設定

ユーザの追加/編集画面に「2要素認証を使う」チェックボックスが追加されます。  
このチェックをONにして保存すると、QRコードが表示されます。  
これをスマホのGoogleAuthenticatorアプリで読み取りましょう。

## 注意

2要素認証を使う設定にした上でQRコードを読み取るのを忘れてサインアウトするともうそのユーザではログインできなくなります。

## 画面イメージ

![ログイン](https://raw.githubusercontent.com/stsap/stsap.github.io/master/images/2015-01-15_1.png)

![ユーザ設定](https://raw.githubusercontent.com/stsap/stsap.github.io/master/images/2015-01-15_2.png)

## TODO

* Auth::GoogleAuthenticator からの脱却
* スマホ紛失時用の救済
* 他の2要素認証システムへの対応([Yahoo](http://id.yahoo.co.jp/security/otp.html)とかSMSとか？)

まだやりたい事はあるけどとりあえず一段落。  
激しくアルファクオリティなのでご利用はオススメしませんよっと。
