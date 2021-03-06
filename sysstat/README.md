# sysstatについて
サーバのリソース情報をバイナリで収集、管理するツールです。  
情報の種類としては、CPUやメモリ使用率、トラフィック量、ディスクI/Oなど様々な情報が取得できます。

## install
```
# yum -y install sysstat
```

## とりあえず試してみる
```
// ２秒間周期で４回取得
# sar -o sar-test.log 2 4
```
実行結果
```
13:01:00        CPU     %user     %nice   %system   %iowait    %steal     %idle
13:01:02        all      0.00      0.00      0.50      0.00      0.00     99.50
13:01:04        all      0.25      0.00      0.25      0.00      0.00     99.50
13:01:06        all      1.00      0.00      0.00      0.50      0.00     98.50
13:01:08        all      0.50      0.00      0.25      0.00      0.00     99.25
Average:        all      0.44      0.00      0.25      0.13      0.00     99.18
```

## 主要コマンド
|コマンド|内容|
|:-----------|------------|
|sar -q|loadaverage|
|sar -u|CPU使用率|
|sar -b|I/O|
|sar -r|メモリとスワップ使用率|  
|sar -s time|指定時間以降のデータ|  
|sar -e time|指定時間までのデータ|  
|sar -f /var/log/sa/sa01| 日付別の過去データ|

## sar -q (LoadAverage)
データ取得
```
Linux 4.9.184-linuxkit (ebd7455b628a)   02/03/20        _x86_64_        (2 CPU)

06:56:20      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
06:56:25            0       351      0.00      0.00      0.09         0
06:56:30            0       351      0.00      0.00      0.09         0
06:56:35            0       351      0.00      0.00      0.09         0
06:56:40            0       351      0.00      0.00      0.09         0
06:56:45            0       351      0.00      0.00      0.09         0
06:56:50            0       351      0.00      0.00      0.08         0
06:56:55            0       351      0.00      0.00      0.08         0
06:57:00            0       351      0.00      0.00      0.08         0
06:57:05            0       351      0.00      0.00      0.08         0
```
LoadAverageの上昇が見られたら基本的に、以下の2点に原因は絞られる。
* CPU負荷（プロセス不足）
* I/O負荷

## sar -u (CPU使用率)
データ取得
```
Linux 4.9.184-linuxkit (ebd7455b628a)   02/03/20        _x86_64_        (2 CPU)

06:47:22        CPU     %user     %nice   %system   %iowait    %steal     %idle
06:47:23        all      0.50      0.00      0.50      0.00      0.00     99.00
06:47:24        all      0.00      0.00      0.50      0.00      0.00     99.50
06:47:25        all      0.00      0.00      0.50      0.00      0.00     99.50
06:47:26        all      0.50      0.00      0.99      0.00      0.00     98.51
06:47:27        all      0.50      0.00      1.00      0.00      0.00     98.50
06:47:28        all      0.51      0.00      0.00      0.51      0.00     98.99
06:47:29        all      0.50      0.00      0.50      0.00      0.00     99.00
06:47:30        all      0.00      0.00      0.00      0.00      0.00    100.00
06:47:31        all      1.00      0.00      0.00      0.00      0.00     99.00
06:47:32        all      0.50      0.00      0.00      0.00      0.00     99.50
06:47:33        all      0.00      0.00      0.00      0.00      0.00    100.00
06:47:34        all      0.50      0.00      1.00      0.50      0.00     98.01
06:47:35        all      0.50      0.00      0.00      0.00      0.00     99.50

06:47:36        all      0.00      0.00      0.99      0.00      0.00     99.01
Average:        all      0.37      0.00      0.41      0.07      0.00     99.15
```

|モード|内容|
|:-----------|------------|
|%user|アプリケーション（ユーザプロセス）が使用している状態|
|%system| カーネル（OSなど）が使用している状態 |
|%iowait| ディスクI/O待ち状態|
|%idle | CPUが何の処理もしない待機状態（I/O待ちの時間は除く）|

### sar -u の調査でポイントになる箇所  
* %user or %systemが高い
  * CPUボトルネック（もしくはメモリ不足）
  * %userならアプリケーション側
  * %systemならカーネル側。
* %iowaitが高い
  * ディスクI/Oボトルネック。

## sar -b (I/O)
データ取得
```
Linux 4.9.184-linuxkit (ebd7455b628a)   02/03/20        _x86_64_        (2 CPU)

07:15:43          tps      rtps      wtps   bread/s   bwrtn/s
07:15:44         2.00      0.00      2.00      0.00     24.00
07:15:45         0.00      0.00      0.00      0.00      0.00
07:15:46         0.00      0.00      0.00      0.00      0.00
07:15:47         0.00      0.00      0.00      0.00      0.00
07:15:48         0.00      0.00      0.00      0.00      0.00
07:15:49         0.00      0.00      0.00      0.00      0.00
07:15:50         2.00      0.00      2.00      0.00     24.00
07:15:51         0.00      0.00      0.00      0.00      0.00
07:15:52         0.00      0.00      0.00      0.00      0.00
07:15:53         0.00      0.00      0.00      0.00      0.00
07:15:54         0.00      0.00      0.00      0.00      0.00
07:15:55         2.00      0.00      2.00      0.00     24.00
07:15:56         0.00      0.00      0.00      0.00      0.00
07:15:57         0.00      0.00      0.00      0.00      0.00
07:15:58         0.00      0.00      0.00      0.00      0.00
07:15:59         0.00      0.00      0.00      0.00      0.00
^C

07:16:00         0.00      0.00      0.00      0.00      0.00
Average:         0.36      0.00      0.36      0.00      4.37
```
|項目|内容|
|:-----------|------------|
|tps| 秒間I/Oリクエスト 数の合計。|
|rtps| 秒間読み込みIOリクエスト数の合計。|
|wtps| 秒間書き込みIOリクエスト数の合計。|
|bread/s| 秒間読み込み（ブロック単位）IOリクエストのデータ量の合計。|
|bwrtn/s| 秒間書き込み（ブロック単位）IOリクエストのデータ量の合計。|

## sar -r (メモリとスワップの使用状況)
データ取得
```
Linux 4.9.184-linuxkit (ebd7455b628a)   02/03/20        _x86_64_        (2 CPU)

07:46:46    kbmemfree kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
07:46:47      1175900    871108     42.56      8600    514408   2692172     86.97    414764    338588         8
07:46:48      1175900    871108     42.56      8600    514408   2692172     86.97    414764    338588         8
07:46:49      1175900    871108     42.56      8600    514408   2692172     86.97    414764    338588         8
07:46:50      1175900    871108     42.56      8600    514408   2692172     86.97    414764    338588         8
07:46:51      1175900    871108     42.56      8600    514408   2692172     86.97    414764    338588         8
07:46:52      1175900    871108     42.56      8608    514400   2692172     86.97    414764    338588         8
07:46:53      1175900    871108     42.56      8608    514408   2692172     86.97    414768    338588         4
07:46:54      1175900    871108     42.56      8608    514408   2692172     86.97    414768    338588         4
07:46:55      1175900    871108     42.56      8608    514408   2692172     86.97    414768    338588         4
07:46:56      1175900    871108     42.56      8608    514408   2692172     86.97    414768    338588         4
07:46:57      1175900    871108     42.56      8616    514408   2692172     86.97    414788    338588        20
07:46:58      1175900    871108     42.56      8616    514408   2692172     86.97    414692    338588         0
^C
07:46:58      1175900    871108     42.56      8616    514408   2692172     86.97    414692    338588         0
Average:      1175900    871108     42.56      8607    514407   2692172     86.97    414756    338588         6
```
|項目|内容|
|:-----------|------------|
|kbmemfree| メモリ空き容量(kb)|
|kbmemused| メモリ使用量(kb)|
|%memused| メモリ使用率|
|kbswpfree| スワップ空き容量(kb)|
|kbswpused| スワップ使用量(kb)|
|%swpused| スワップ使用率|

## 参考ページ  
* http://kumonchu.com/linux/sysstat-install-setting/
* https://github.com/sysstat/sysstat
* https://gist.github.com/koudaiii/0c4838eb7dec89dc8cac