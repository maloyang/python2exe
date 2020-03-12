# python2exe

記錄怎麼把python程式包裝成exe檔，讓其它台沒有python環境的電腦可以執行程式。

- 一開始我都是使用py2exe這一個套件，不過最近(2020/02/19)再次使用，發現怎麼包都有問題，由x64的python改成x32的python仍然不行…
- 後來改用pyinstaller就可以了，但在我有使用到SQL相關套件時，因為會引入dll的東西，又造成程式不能執行，因此記錄一下解決的過程

----
## pyinstaller
一開始我是在x64的python環境，後來因為想要讓程式可以在x64, x86都可以執行，因此以下的操作都是以 x86 python 操作

- 安裝指令: pip install pyinstaller

### 以 console_exe_1 資料夾為例

- 因為只有一個 print('hello')最為簡單，只需要command line下指令 `pyinstaller --onefile console.py`
- 產生的執行檔會在 dist 資料夾中，裡面我有放一個 `run.bat` 方便執行程式，並看執行結果

### 如果import比較多東西時
[ref](https://blog.csdn.net/ddxwltan/article/details/81835339)

- 這一個例子我引入比較多
```
from sqlalchemy import create_engine
import paho.mqtt.client as mqtt
import time, json, datetime
```

- 因為sqlalchemy我使用時會隱含使用到pymssql的套件，`db_url = "mssql+pymssql://abc:123@1.2.3.4:1433/mydb"` ，但pyinstaller不會知道，所以包裝會成功，但是執行時會有 `ModuleNotFoundError: No module named 'pymssql'` 的錯誤產生
- 解決的方式試了很多，但最有效的莫過於直接把你有隱含用到的套件再引入一次，如下:
```
import pymssql
pymssql.__version__
```
因為只是要讓pyinstaller知道我們有用到這套件，請它包進去，因此只需要執行取得版本號即可

- 執行包裝的指令 `pyinstaller --onefile dbtest.py` 就可以了

- 如果需要加入自己的icon的話，可以改下指令: `pyinstaller --onefile dbtest.py -i icon.ico`，就可以得到

- 而icon檔要怎麼轉呢? 可以到[這邊](https://icoconvert.com/)上傳你的圖檔，轉成可使用的.ico檔

- 如果要在windows排程中執行, 不想要有視窗出來干擾工作，可以改下這指令處理：`pyinstaller --onefile dbtest.py -i icon.ico -w`
  - 後來我在win10下產生沒視窗的程式會被內建的defender防毒視為木馬，所以沒辨法執行，因此我使用排程執行時改為「未登入也執行」，這樣它就會沒有視窗，自己靜默在背景執行程式了!
  
  
----
## py2exe
這個晚點再補上，因為本來可以用的方法現在都怪怪的不能使用了…
