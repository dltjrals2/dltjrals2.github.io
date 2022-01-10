---
title: "[WebScraping] PyQt5 구조 및 GUI 사용방법"

categories:
  - WebScraping
tags:
  - [Crawler, Python, Selenium, PyQt5, Qt Designer]

toc:  true
toc_sticky: true

date: 2022-01-07
last_modified_at: 2022-01-07
---

📌**<u>개발 환경</u>**  
```
OS = Window and Mac
Python = 3.8 (Selenium)
IDE : Pycharm
PyQt
Qt Designer
```  

---
📌**<u>전체코드는 https://github.com/dltjrals2/WebScrapingTool 에 있으니, 참고해주세요.</u>**  

## PyQt5 프로그램 구조  

```python
# PyQt5 Library 추가
from PyQt5 import QtWidgets
from PyQt5 import uic
from PyQt5.QtCore import *
from PyQt5.QtGui import *

# UI Window Class
class MainWindow(QtWidgets.QDialog):

    def __init__(self):
        super().__init__()
        # Init / Instance Property

# Main
if __name__ == "__main__":
    app = QtWidgets.QApplication(sys.argv)
    var_MainWindow = MainWindow()
    sys.exit(app.exec())
```  

- `def __init__(self)` : 초기화/인스턴스 속성 지정  
- `super().__init__()` : MainWindow()의 기반 클래스 QtWidgets.QDialog를 초기화  
- `__name__` : 해당 Module이 Import가 아닌 자기 자신에서 프로그램을 실행시켰을 경우 실행  
- `sys.exit(app.exec())`  
-- sys.exit(0)로 받으면 python은 루프에서 빠져나와 정상종료  
-- app.exec() : 프로그램이 꺼지지 않게 무한루프를 만들어준다  
-- 즉, 프로그램을 무한루프로 돌려 실행시키고, 프로그램 내에서 정상적으로 명령을 수행 후, 정상 종료하면 0을 반환하여 무한루프를 빠져나오는 방식  

## PyQy5 GUI 화면 전환  

```python
# PyQt5 Library 추가
from PyQt5 import QtWidgets
from PyQt5 import uic
from PyQt5.QtCore import *
from PyQt5.QtGui import *

# Function Import
import os

## python실행파일 디렉토리
BASE_DIR = os.path.dirname(os.path.abspath(__file__))

# UI Window Class
class MainWindow(QtWidgets.QDialog):
    global Var_LoginAuthenWindow

    def __init__(self):
        super().__init__()
        self.ui = uic.loadUi(BASE_DIR + r'\DataSelect.ui')
        self.ui.LoginAuthenButton.clicked.connect(self.openLoginWindodw)

        self.ui.show()

    def openLoginWindow(self):
        self.ui.close()
        Var_LoginAuthenWindow.ui.show()

# UI Login class
class LoginAuthenWindow(QtWidgets.QDialog):
    global Var_MainWindow

    def __init__(self):
        super().__init__()
        self.ui = uic.loadUi(BASE_DIR + r'\Login.ui')
        self.ui.Login_Button.clicked.connect(self.checkLoginInfo)

    def checkLoginInfo(self):
        # Login Process
        # ...
        self.ui.close()
        Var_MainWindow.ui.show()

if __name__ == "__main__":
    app = QtWidgets.QApplication(sys.argv)
    Var_LoginAuthenWindow = LoginAuthenWindow()
    Var_MainWindow = MainWindow()
    sys.exit(app.exec())
```  
- `BASE_DIR` : pyinstaller을 통해서 .exe 파일로 패키징을 진행할 때, UI 경로를 .py가 있는 곳으로 지정하기 위해 os를 이용한 경로 지정
- `MainWindow` : 데이터 선택하는 Main GUI  
- `self.ui = uic.loadUi(BASE_DIR + r'\DataSelect.ui')` : 지정한 경로에있는 'DataSelect.ui'를 MainWindow UI 지정  
- `self.ui.LoginAuthenButton.clicked.connect(self.openLoginWindodw)` : LoginAuthenButton 클릭 시, openLoginWindow 함수 실행  
- `LoginAuthenWindow` : 로그인 기능을 지원하는 Main GUI  
- `self.ui = uic.loadUi(BASE_DIR + r'\Login.ui')` : 지정한 경로에 있는 'Login.ui'를 LoginAuthenWindow UI 지정  
- `self.ui.Login_Button.clicked.connect(self.checkLoginInfo)` : Login_Button 클릭 시, checkLoginInfo 함수 실행  
- `self.ui.close()` : 현재 보여지고 있는 UI 창 닫기  
- `Var_LoginAuthenWindow.ui.show()` : Login UI 창 열기  
- `Var_MainWindow.ui.show()` : DataSelect UI 창 열기  

---
**🐢개발 완료 된 프로그램에 대한 개념 및 내용을 정리하고자 하는 목적으로 작성되었습니다. 궁금하신점은 댓글 남겨주세요.🐢**
{: .notice--primary}

감사합니다.😊
