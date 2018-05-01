# Ruby on Railsの勉強メモ

## 概要

udemyでRuby on Railsについての教材を買ってみた。
学習のメモを記載する。

## 環境について

- Ubuntu 18.04
- Ruby on Rails --version 4.2.8

※動画ではMac & HomeBrewの環境で環境構築をしていた

**Ubuntuは適宜aptコマンドに差し替えて実施**

## 1日目_2018/04/30

環境構築は２時間程度で完了。x

### 環境構築

#### Rubyのインストール

デフォルトで入っていない場合は下記でインストール

```
apt install Ruby ruby-dev
```

※gemでRailesインストールの際に**ruby-devがないとエラーになる**ので注意

バージョン確認

```
y_shirai@x230:~/Downloads$ ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux-gnu]
y_shirai@x230:~/Downloads$
```

#### rbenvのインストール

複数のRubyのバージョンを切り替えたり出来る

```
sudo apt install rbenv
```

rbenvの初期設定を.bashrcの末尾に追加

```
echo 'eval "$(rbenv init -)"' >> ~/.bashrc

# bashrcの再読込
. ~/bashrc
```

#### 複数のrubyをインストール

rbenvを使ってrubyをインストール

```
# インストールできる一覧を確認
rbenv install -l

# 特定バージョンをインストール
## ダウンロードとインストールなので少し時間がかかる
rbenv install 2.4.0

# インストールされているバージョンの確認
$ rbenv versions
* system (set by /home/y_shirai/.rbenv/version)
  2.4.0

# バージョンの切り替え
rbenv local 2.4.0

```

※バージョン2.3.0のインストールは何故か失敗するので2.4.0と2.5.0で学習をすすめる

#### Railsのインストール

Rubyが入っていればgem（パッケージ管理ツール？ pipみたいなもの）が使える

```
# Railsインストール
sudo gem install rails --version "4.2.8"

# バージョン確認
$ rails -v
Rails 4.2.8

```

### はじめてのRailsプロジェクト作成と動作確認

作業ディレクトリを作成後プロジェクト生成

```
mkdir ~/railsstudy && cd ~/railsstudy

$ ${projectname}は任意のプロジェクト名

rails new ${projectname}

cd ${projectname}

## Gemfileの書き換え
## JavaScript Runtimeが入ってないとrailsサーバーがスタートできない
sed -i -e s/"# gem 'therubyracer'"/"gem 'therubyracer'"/ Gemfile

## JavaScript Runtimeを導入
bundle install

## Serverのスタート
rails s

## ブラウザで確認
http://localhost:3000 にアクセス。

Welcome aboard　なるページが開けばOK

```

※エラーの参考： https://qiita.com/noraworld/items/d92cca9bb449b48a97aa

## 2日目_2018/05/01

### Ruby言語

- スクリプト言語
  - v1.9からは実行時にバイトコードという仮想マシンで変更後、コンピュータが解釈できる機械語への変換が行われる。
  - ユーザーはプログラムの実行時にバイトコード変換を意識することはない
- すべての命令やデータをオブジェクトとして扱う（Javaみたいなイメージ？）
- 日本人のまつもとひろゆき氏が開発者 = 日本語ドキュメントが充実？
- **v2.0以降は文字コードはUTF-8で保存する必要がある**

### Rubyの実行形式

- エディタで拡張子.rbファイルを作成
- irbコマンドで対話形式で実行
- サーバー上で実行
  - rbファイルをサーバーに配置して実行
  - erbファイルをHTMLファイルに埋め込む
- JRubyというものを使えばデスクトップアプリも作れるらしい

### Rubyリファレンス

[Rubyリファレンスドキュメント](https://www.ruby-lang.org/ja/documentation/)

仕様を確認できる

### Ruby言語の記法詳細

#### すべてがオブジェクト

- 変数もオブジェクト
- オブジェクトのテンプレートをクラスと呼ぶ（Javaのクラスとは違う？）
- クラスの実態をインスタンスとも呼ぶ。インスタンスも当然オブジェクトである
- 文字列などのデータもオブジェクト

#### クラスの継承について

- Rubyは1回のみ継承可能で多重継承は不可能。つまり親子関係のみになる。

#### メソッド（処理）について

- メソッドは処理のことを指す（Javaと同じ感覚）

下記のようにかける

```Ruby
#メソッド 引数
ptus "HelloWorld"
```

実行の仕方

ターミナルからrubyコマンドの引数にファイル名を指定する

```
$ ruby ${filename}.rb
```

#### 変数について

変数

- 変数はオブジェクトとして扱われる（Javaの様に型は使わない？)
- メソッドを代入することも出来る
- 変数を使うときは**変数名を書けば良い（$などはつけない）**
- 代入演算子は=で表現

変数の種類

|記載|種類|補足|
|:---|:---|:---|
|hoge|ローカル変数|同じクラスからのみ参照|
|$hoge|グローバル変数|別クラスから参照可能|
|@hoge|インスタンス変数|後で書く|
|@@hoge|クラス変数|後で書く|
|HOGE|定数（コンスタント）|再代入できない|

#### 文字列や値について

- '（シングルクォート）は式や変数を展開**しない**
- "（ダブルクォート）は式や変数を展開**する**
- "で式や変数を使う場合は**#{}**を使う

test