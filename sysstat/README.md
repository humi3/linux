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

## 参考ページ  
* http://kumonchu.com/linux/sysstat-install-setting/
* https://github.com/sysstat/sysstat