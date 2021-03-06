---
title: "[WebScraping] ๊ฐ๋ฐ UI ์ค๋ช"

categories:
  - WebScraping
tags:
  - [Crawler, Python, Selenium, PyQt5, Qt Designer]

toc:  true
toc_sticky: true

date: 2022-01-04
last_modified_at: 2022-01-07
---

๐**<u>๊ฐ๋ฐ ํ๊ฒฝ</u>**  
```
OS = Window and Mac
Python = 3.8 (Selenium)
IDE : Pycharm
PyQt
Qt Designer
```  

---
๐ ์ ์ฒด์ฝ๋ ์ฃผ์ : <https://github.com/dltjrals2/WebScrapingTool>  

## Data Select UI  

![image](https://user-images.githubusercontent.com/37467408/147997855-46ff5659-574a-4b0f-b922-e650b4e1bfd4.png)  

### Time Setting(QDateEdit, QCombobox)  

![image](https://user-images.githubusercontent.com/37467408/147998473-91c0de1d-5a1b-4c2f-bcad-22463e3601aa.png)  
- ๋, ์, ์ผ ๊ธฐ๊ฐ ์ค์ ์ ์ํ `QDateEdit` > objectName : FromDate, ToDate ๋ก ์ค์   

![image](https://user-images.githubusercontent.com/37467408/147998560-0fb380a2-6829-48aa-80dc-6681e375252a.png)  
- ์๊ฐ, ๋ถ ๊ธฐ๊ฐ ์ค์ ์ ์ํ `QCombobox` > objectName : FromHour, FromMinute, ToHour, ToMinute ๋ก ์ค์   
- ๊ฐ๊ฐ ๋ฐ์ดํฐ๋ก FromHour๊ณผ ToHour์๋ 0 ~ 23์ด, FromMinute์๋ 0, 10, 20, 30, 40, 50์ด ์๊ณ  ToMinute์๋ 9, 19, 29, 39, 49, 59 ๋ก ๋ฐ์ดํฐ ์ค์ (์ ๋ ์ ๋ฐ์ดํฐ ํ์์ผ๋ก ๋ง์ถฐ ์งํํด์ ์ ๋ฐ์ดํฐ๋ก ์ถ๊ฐํ์ต๋๋ค.)  

### Login, Refresh, Update, ์ถ์ถ Button(QPushButton)  

![image](https://user-images.githubusercontent.com/37467408/147998798-94f269f9-8241-46fb-a6f2-1fd222f75243.png)  
- Login : ๋ก๊ทธ์ธ UI๋ก ์ ํํ  ์ ์๋๋ก ํ๋ ๋ฒํผ > objectName : LoginAuthenButton  
- Refresh : ๋ฐ์ดํฐ๋ฅผ ์๋ฐ์ดํธ ํ ํ, ๋ฐ์ดํฐ ๋ฆฌ์คํธ๋ Button Enable ๊ธฐ๋ฅ ์คํ์ ์ํ ๋ฒํผ > objectName : RefreshButton  
- Update : ํ๊ฒ ์น์ ์ ์ ํด ๋ฐ์ดํฐ์ ๋ฆฌ์คํธ๋ฅผ ๊ฐ์ ธ์ ์๋ `QListWidget`์ ์ถ๊ฐํ  ์ ์๋๋ก ๋ฐ์ดํฐ ์ฒ๋ฆฌํ๋ ๋ฒํผ > objectName : UpdateButton  

![image](https://user-images.githubusercontent.com/37467408/147999039-0b5e9bab-03f7-4456-b8d8-8df6e14e43fc.png)  
- ์ถ์ถ : ์ `QListWidget`์์ ์ ํ๋์ด์ง ๋ฐ์ดํฐ์ ์ ์ฅ๋์ด์๋ ๋ก๊ทธ์ธ ์ ๋ณด๋ก ํ๊ฒ ์น์ด ์ ๊ทผํด์ ๋ฐ์ดํฐ๋ฅผ ์์๋ก ๊ฐ์ ธ์ค๋ ๋ฒํผ > objectName : extractButton  

### Status Label(QLabel)  

![image](https://user-images.githubusercontent.com/37467408/147999799-03a18fff-f61e-4e73-90c8-c7e0d392a356.png)  
- ํ๋ก๊ทธ๋จ ์งํ๊ณผ์ ์ ๋ฐ๋ฅธ ๊ฐ์ด๋ ๋ฐ ์ค๋ฅ์ ๋ํ ํ์(Print ๊ธฐ๋ฅ) > objectName : StatusLabel  

### DataList Widget(QListWidget, QCombobox)  

![image](https://user-images.githubusercontent.com/37467408/147999170-956c4410-ce84-41cf-9e38-b11d2f2a5570.png)  
- ์นดํ๊ณ ๋ฆฌ๋ก ๋ถ๋ฅ๋ฅผ ์ํ `QCombobox` ์ค์ , ํด๋น `QCombobox`์ ๋ด์ฉ์ Update ๋ฒํผ์ ๋๋ฅด๋ฉด ๋ณ๊ฒฝ์ด ๋์ด์ง๋๋ค. > objectName : GroupBox
- ํ์ฌ ๋ณด์ฌ์ค ์ ์๋ ๋ฐ์ดํฐ๋ฅผ ๋์ด์ฃผ๋ `QListWidget`, ํด๋น Widget์ ๋ฐ์ดํฐ์ ๋ด์ฉ์ Update ๋ฒํผ์ ๋๋ฅด๋ฉด ๋ณ๊ฒฝ์ด ๋์ด์ง๋๋ค. > objectName : GroupListWidget  

![image](https://user-images.githubusercontent.com/37467408/147999492-1c4577cb-ec08-4c8d-994f-87d8cbcfbe2b.png)  
- ์์์ ๋ฐ์ดํฐ๋ฅผ ์ ํ ํ, ๋๋ธ ํด๋ฆญํ๋ฉด ์ค๋ฅธ์ชฝ `QListWidget`์ผ๋ก ๋ฐ์ดํฐ๋ฅผ ๋๊ฒจ ์ ํ๋์ด์ง ๋ฐ์ดํฐ๋ฅผ ํ์ธํ  ์ ์๋๋ก ํ๋ Widget > objectName : SelectListWidget  

## Login UI

![image](https://user-images.githubusercontent.com/37467408/147997875-6452ad16-4bce-4562-932d-6e2180d93f3a.png)  

### ID, Password ์๋ ฅ(QLineEdit)  

![image](https://user-images.githubusercontent.com/37467408/147999395-3e7735a3-7071-4fd6-89eb-55505db502d4.png)  
- ID ์๋ ฅ์ ์ํ `QLineEdit` > objectName : Login_ID  
- Password ์๋ ฅ์ ์ํ `QLineEdit` > objectName : Login_Password  

### Login Button(QPushButton)  

![image](https://user-images.githubusercontent.com/37467408/147999576-84ae9fe2-d181-4e57-b855-65dc95b80ca9.png)  
- ํ๊ฒ ์น์ ์ ๊ทผํด ๋ก๊ทธ์ธ ์ ๋ณด๊ฐ ์ผ์นํ๋์ง ํ์ธ ํ๋ ๋ฒํผ, ๋ง์ ๊ฒฝ์ฐ txt ํ์ผ๋ก ๋ก๊ทธ์ธ ์ ๋ณด ์ ์ฅ > objectName : Login_Button  

### StatusLabel, ProgressBar(QLabel, QProgressBar)  

![image](https://user-images.githubusercontent.com/37467408/147999683-de4f16b2-5576-42f2-8972-e4926760fa65.png)  
- ๋ก๊ทธ์ธ ์ํ์ ์น์ ์ ๊ทผํด ๋ก๊ทธ์ธ ์ฒ๋ฆฌ๊ฐ ๋๋ ๊ณผ์ ์ ProgressBar์ ํตํด ํ์ธ > objectName : Login_Status, Login_ProgressBar  


---
**๐ข๊ฐ๋ฐ ์๋ฃ ๋ ํ๋ก๊ทธ๋จ์ ๋ํ ๊ฐ๋ ๋ฐ ๋ด์ฉ์ ์ ๋ฆฌํ๊ณ ์ ํ๋ ๋ชฉ์ ์ผ๋ก ์์ฑ๋์์ต๋๋ค. ๊ถ๊ธํ์ ์ ์ ๋๊ธ ๋จ๊ฒจ์ฃผ์ธ์.๐ข**
{: .notice--primary}

๊ฐ์ฌํฉ๋๋ค.๐
