footer: Python入門者の集い #6 2018/01/16 - Ozawa Shuhei ( @oza_shu )
slidenumbers: true
autoscale: true

# [fit] **PyQではじめるPython**

---

# **自己紹介**<br>
![left,inline 50%](https://pbs.twimg.com/profile_images/930276892141170693/7WMcvMuZ_400x400.jpg)

- 小沢周平
- @oza_shu
- MSP業界 WEBサービスのサーバ運用

--- 

# **PyQとは**
- 写経ベースで課題を解いて覚える
- 過去の課題で習った文法も反復して学習できる
- 学習カレンダーができた

---

# **学習できる内容**

- 基本的な文法
- Webアプリ(Django)
- データ処理(pandas)
- 機械学習

---

# **PythonでLinuxサーバと仲良くなる**
- アプリを動かしているプロセスについてもっと知りたい
- どうやってアプリにアクセスしているのか知りたい

---

# **アプリを動かしているプロセスについてもっと知りたい**

- OSはプロセスを、プロセスに割り振られたPIDをみて数値として扱う
- プロセスが開いたファイルにはファイルディスクリプタが割り当てられる

---

# **コード例(1) PIDをみてみよう**

```python
# 現在のプロセスを返す
import os
print ("My pid:", os.getpid())
```
```python
$ python3 3_processes_have_pid.py
My pid: 10592
```

---
# **psコマンドでも確認できる**

```
ps aux |grep zsh
USER  PID     %CPU %MEM VSZ     RSS   TT  STAT  STARTED  TIME     COMMAND
root  74749   0.0  0.0  4296948 3016 s005  Ss    2:39AM  0:00.11 -zsh
root  73439   0.0  0.0  4296948 3028 s003  Ss    2:19AM  0:00.11 -zsh
root  63923   0.0  0.0  4296948 2980 s001  Ss+  11:15PM  0:00.10 -zsh
```

# **PIDからSWAP使用量が多いプロセスを特定**

```
$ grep VmSwap /proc/*/status |sort -k2 -r |head -n 
/proc/2121/status:VmSwap:         158284 kB
/proc/16520/status:VmSwap:        154516 kB
/proc/18192/status:VmSwap:        141116 kB
```

---

## **コード例(2) ファイルディスクリプタ**

```python
# ファイルディスクリプタが割り当てられているかみてみよう
if __name__ == "__main__":
    with open("/etc/passwd") as passwd:
         print(passwd.fileno())
```

```python
# ファイルを閉じると消える
$ python3 file_descriptors.py
3
```

---

## **特別なファイルディスクリプタ**

* 標準入力 → 0
* 標準出力 → 1
* 標準エラー出力 → 2

```python
>>> sys.stdin.fileno()
0
>>> sys.stdout.fileno()
1
>>> sys.stderr.fileno()
2
```

---

# **曖昧だった書き方を理解できるようになった**

:ok_woman:

```
command >/dev/null 2>&1
```

:no_good:

```
command 2>&1 >/dev/null
```

---

# **fork()気になってくる**

プロセスがForkされるってどういうことだっけ?
子プロセスは親プロセスで使われている全てのメモリのコピーを引き継ぐ
ファイルディスクリプタ(ソケット)も

- socket,osモジュールなどでpreforkサーバも書ける
- コピー・オン・ライト（CoW、Copy on Write）
- 何でゾンビプロセスってなるんだっけ
- プロセス間通信
   - パイプ処理,ストリーム,メッセージ
   - シグナル処理
   - プロセスグループ,セッショングループ

---
# **まとめ**

- PyQは写経を通しての学習なので、プログラミング初めたばかりの人によさそう
- Pythonを通じてLinuxと仲良くなれる
- プロセスがどう実行されているのかちゃんと理解する助けにもなる
