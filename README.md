# python2exe

記錄怎麼把python程式包裝成exe檔，讓其它台沒有python環境的電腦可以執行程式。

- 一開始我都是使用py2exe這一個套件，不過最近(2020/02/19)再次使用，發現怎麼包都有問題，由x64的python改成x32的python仍然不行…
- 後來改用pyinstaller就可以了，但在我有使用到SQL相關套件時，因為會引入dll的東西，又造成程式不能執行，因此記錄一下解決的過程

## pyinstaller
一開始我是在x64的python環境，後來因為想要讓程式可以在x64, x86都可以執行，因此以下的操作都是以 x86 python 操作

### 以 console_exe_1 資料夾為例

- 因為只有一個 print('hello')最為簡單，只需要command line下指令 `pyinstall --onefile console.py`
- 產生的執行檔會在 dist 資料夾中，裡面我有放一個 `run.bat` 方便執行程式，並看執行結果

