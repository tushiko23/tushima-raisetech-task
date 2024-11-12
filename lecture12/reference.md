# 今回のサンプルコンフィグの解説

```
version: 2.1
orbs:
  python: circleci/python@2.0.3
jobs:
  cfn-lint:
    executor: python/default
    steps:
      - checkout
      - run: pip install cfn-lint
      - run:
          name: run cfn-lint
          command: cfn-lint -i W3002 -t cloudformation/*.yml
            
workflows:
  raisetech:
    jobs:
      - cfn-lint
```

* version 2.1 

Circleciを動かすymlファイルのバージョン。最新は2.1。

* cfn-lint

AWS CloudFormation テンプレートの検証に利用できるオープンソースツール。pythonパッケージである。pythonで書かれており、使用するにはpython環境が必要である。

* orbs

CircleCI の設定をパッケージとして公開し、再利用するための仕組み。
今回は、Cfn-lintを使用するために、orbsにてpython環境を作り、pythonパッケージをインストールできるようにするために使用する。

* jobs

CircleCIでコマンドやスクリプトを実行するためのすべてのステップを含んでいる重要な要素。

* executor

ジョブが実行される環境を指定する。今回は、python/defaultを指定し、python環境を指定。定義されるジョブは固有のexecutorで実行される。

* steps

ジョブを完了させるために実行する必要があるアクション。通常は、実行可能なコマンドの集合。

* checkoutステップ

ソースコードをリポジトリから取得するステップ。

* runステップ

具体的にどんな動作を行うかを記述するステップ。
今回は、pipでcfn-lintをインストールする動作と、cfn-lintコマンド-iオプション(ignore:無視する)でw3002(警告コード3002)を無視するオプションをつけて、cloudformationディレクトリ下の.ymlファイルを確認するコマンドを実行します。

* w3002コードの意味

プロパティがpackageコマンドでのみ機能する場合に警告を出す。ここでのプロパティはCloudFormationのymlファイルのテンプレートをさす。

* name

nameを付ける意味は任意のジョブの名前をつけて、ログに表示されるようにし、どのステップでなにをしているかわかりやすくするために付けています。
今回は、run cfn-lintという名前で表示されるように設定しています。

* workflows

ジョブのリストとその実行順を定義します。ここでは、raisetechというワークフロー名が作成され、1番目にcfn-lintというジョブを定義します。

参考サイト

[circleci公式youtube1](https://www.youtube.com/watch?v=ttavquLE4Fk)

[circleci公式youtube2](https://www.youtube.com/watch?v=cOHKRYgdzDY)

[circleci公式youtube3](https://www.youtube.com/watch?v=rCh6jaJ8gV8)

[CircleCI を使いこなす！ JOBの設定方法を徹底解説！！【CircleCI入門】](https://www.youtube.com/watch?v=NWjNn2DLcPM)

[CI/CDプロセスにCloudFormationを本気導入するために考えるべきこと](https://speakerdeck.com/hamadakoji/cdpurosesunicloudformationwoben-qi-dao-ru-surutamenikao-erubekikoto?slide=55)

[Rules](https://github.com/aws-cloudformation/cfn-lint/blob/main/docs/rules.md)

[pipの使い方](https://envader.plus/course/8/scenario/1072)
["This environment is externally managed"のエラーが出たとき、python仮想環境をつかう。](https://note.com/lucky_gerbil210/n/n0de8721c05bc)

[pip で error:  externally-managed-environment が出てパッケージがインストールできない](https://scrapbox.io/bbr-software-memo/pip_%E3%81%A7_error:__externally-managed-environment_%E3%81%8C%E5%87%BA%E3%81%A6%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E3%81%8C%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%A7%E3%81%8D%E3%81%AA%E3%81%84)