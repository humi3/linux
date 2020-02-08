# linux 立ち上げ
 勉強用にcentos7を使用

## 設定内容
 Dockerfileを参照

## build
`docker build -t systat-image ./`

## run
`docker run -v C:\project\linux\docker\log:/home/sysstat-log --privileged --name sysstat-test -idt systat-image /bin/bash`

## attach
`docker attach sysstat-test`

## コンテナから抜けるとき
ctrl + p + q

## memo

## 参考ページ
* https://qiita.com/pottava/items/452bf80e334bc1fee69a