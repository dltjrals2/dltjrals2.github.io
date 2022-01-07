---
title: "[WebScraping] 개발 UI 설명"

categories:
  - WebScraping
tags:
  - [Crawler, Python, Selenium, PyQt5, Qt Designer]

toc:  true
toc_sticky: true

date: 2022-01-04
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

## Data Select UI  

![image](https://user-images.githubusercontent.com/37467408/147997855-46ff5659-574a-4b0f-b922-e650b4e1bfd4.png)  

### Time Setting(QDateEdit, QCombobox)  

![image](https://user-images.githubusercontent.com/37467408/147998473-91c0de1d-5a1b-4c2f-bcad-22463e3601aa.png)  
- 년, 월, 일 기간 설정을 위한 `QDateEdit` > objectName : FromDate, ToDate 로 설정  

![image](https://user-images.githubusercontent.com/37467408/147998560-0fb380a2-6829-48aa-80dc-6681e375252a.png)  
- 시간, 분 기간 설정을 위한 `QCombobox` > objectName : FromHour, FromMinute, ToHour, ToMinute 로 설정  
- 각각 데이터로 FromHour과 ToHour에는 0 ~ 23이, FromMinute에는 0, 10, 20, 30, 40, 50이 있고 ToMinute에는 9, 19, 29, 39, 49, 59 로 데이터 설정(저는 위 데이터 형식으로 맞춰 진행해서 위 데이터로 추가했습니다.)  

### Login, Refresh, Update, 추출 Button(QPushButton)  

![image](https://user-images.githubusercontent.com/37467408/147998798-94f269f9-8241-46fb-a6f2-1fd222f75243.png)  
- Login : 로그인 UI로 전환할 수 있도록 하는 버튼 > objectName : LoginAuthenButton  
- Refresh : 데이터를 업데이트 한 후, 데이터 리스트나 Button Enable 기능 실행을 위한 버튼 > objectName : RefreshButton  
- Update : 타겟 웹에 접속 해 데이터의 리스트를 가져와 아래 `QListWidget`에 추가할 수 있도록 데이터 처리하는 버튼 > objectName : UpdateButton  

![image](https://user-images.githubusercontent.com/37467408/147999039-0b5e9bab-03f7-4456-b8d8-8df6e14e43fc.png)  
- 추출 : 위 `QListWidget`에서 선택되어진 데이터와 저장되어있는 로그인 정보로 타겟 웹이 접근해서 데이터를 엑셀로 가져오는 버튼 > objectName : extractButton  

### Status Label(QLabel)  

![image](https://user-images.githubusercontent.com/37467408/147999799-03a18fff-f61e-4e73-90c8-c7e0d392a356.png)  
- 프로그램 진행과정에 따른 가이드 및 오류에 대한 표시(Print 기능) > objectName : StatusLabel  

### DataList Widget(QListWidget, QCombobox)  

![image](https://user-images.githubusercontent.com/37467408/147999170-956c4410-ce84-41cf-9e38-b11d2f2a5570.png)  
- 카테고리로 분류를 위한 `QCombobox` 설정, 해당 `QCombobox`의 내용은 Update 버튼을 누르면 변경이 되어집니다. > objectName : GroupBox
- 현재 보여줄 수 있는 데이터를 띄어주는 `QListWidget`, 해당 Widget의 데이터의 내용은 Update 버튼을 누르면 변경이 되어집니다. > objectName : GroupListWidget  

![image](https://user-images.githubusercontent.com/37467408/147999492-1c4577cb-ec08-4c8d-994f-87d8cbcfbe2b.png)  
- 위에서 데이터를 선택 후, 더블 클릭하면 오른쪽 `QListWidget`으로 데이터를 넘겨 선택되어진 데이터를 확인할 수 있도록 하는 Widget > objectName : SelectListWidget  

## Login UI

![image](https://user-images.githubusercontent.com/37467408/147997875-6452ad16-4bce-4562-932d-6e2180d93f3a.png)  

### ID, Password 입력(QLineEdit)  

![image](https://user-images.githubusercontent.com/37467408/147999395-3e7735a3-7071-4fd6-89eb-55505db502d4.png)  
- ID 입력을 위한 `QLineEdit` > objectName : Login_ID  
- Password 입력을 위한 `QLineEdit` > objectName : Login_Password  

### Login Button(QPushButton)  

![image](https://user-images.githubusercontent.com/37467408/147999576-84ae9fe2-d181-4e57-b855-65dc95b80ca9.png)  
- 타겟 웹에 접근해 로그인 정보가 일치하는지 확인 하는 버튼, 맞을 경우 txt 파일로 로그인 정보 저장 > objectName : Login_Button  

### StatusLabel, ProgressBar(QLabel, QProgressBar)  

![image](https://user-images.githubusercontent.com/37467408/147999683-de4f16b2-5576-42f2-8972-e4926760fa65.png)  
- 로그인 상태와 웹에 접근해 로그인 처리가 되는 과정을 ProgressBar을 통해 확인 > objectName : Login_Status, Login_ProgressBar  


---
**🐢개발 완료 된 프로그램에 대한 개념 및 내용을 정리하고자 하는 목적으로 작성되었습니다. 궁금하신점은 댓글 남겨주세요.🐢**
{: .notice--primary}

감사합니다.😊
