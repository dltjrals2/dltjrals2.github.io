---
title: "[WebScraping] PyQt5 GUI(QPushButton, QLabel) ์ฌ์ฉ ๋ฐฉ๋ฒ"

categories:
  - WebScraping
tags:
  - [Crawler, Python, Selenium, PyQt5, Qt Designer]

toc:  true
toc_sticky: true

date: 2022-01-10
last_modified_at: 2022-01-10
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

## PyQt5 QPushButton ์ฌ์ฉ ๋ฐฉ๋ฒ  

`QPushButton`์ ์๊ทธ๋  
- self.QPushButton.clicked.connect(ํจ์)  

> ํ๋ก์ ํธ ๋ด ์ฌ์ฉ ์์  

```python
# ์์  1 : QPushButton์ ๋๋ฌ ํจ์ ์คํ
self.ui.QPushButton1.clicked.connect(self.dataUpdate)
self.ui.QPushButton2.clicked.connect(self.RefreshFunc)
```

## PyQt5 QPushButton ๋์ ํจ์  

```python
targetUrl = "URL Address"

class MainWindow(QtWidgets.QDialog):
    def __init__(self):
        super().__init__()
        self.ui = uic.loadUi(BASE_DIR + r'\DataSelect.ui')
        self.ui.UpdateButton.clicked.connect(self.dataUpdate)
        self.ui.RefreshButton.clicked.connect(self.RefreshFunc)

        # ... Init ...

        self.ui.extractButton.clicked.connect(self.dataExtractFunc)

    def dataUpdate(self):
        time.sleep(2)
        # ๋ก๊ทธ์ธ ์ ๋ณด ์ ๋ฌด ํ์
        if os.path.isfile("./user_info.txt"): # ๋ก๊ทธ์ธ ์ ๋ณด ํ์ผ์ด ์กด์ฌ ํ๋์ง ํ์
            with open("./user_info.txt", "r") as login_file:
              login_info = login_file.readlines() # txt ํ์ผ ๋ด์ฉ ์ ๋ถ ์ฝ์ด์ค๊ธฐ
              for i in range(len(login_info)):
                  login_info[i] = login_info[i].replace("\n", "") # ๊ฐํ ๋ฌธ์("\n")๋ฅผ ๊ณต๋ฐฑ์ผ๋ก ์ฒ๋ฆฌ

              # user_info.txt ํ์ผ ํ์
              # 0, 1 > ๋ก๊ทธ์ธ ์ฑ๊ณต ์ฌ๋ถ
              # ID
              # Password

              if login_info[0] == "0": # ๋ก๊ทธ์ธ ์คํจ
                  self.ui.StatusLabel.setText("๋ก๊ทธ์ธ์ ์คํจํ์์ต๋๋ค.\n๋ก๊ทธ์ธ ๋ฒํผ์ ํด๋ฆญํด์ฃผ์ธ์.")
                  return

              # ID and Password Info
              self.LoginID = login_info[1] # txt ์ค ID
              self.LoginPassword = login_info[2] # txt ์ค Password

        driver.get(targetUrl) # Selenium์ผ๋ก Web ๋ฐ์ดํฐ ๊ฐ์ ธ์ค๊ธฐ

        # ์์ ์ด ๋ก๊ทธ์ธํ๊ณ ์ ํ๋ ๋ชฉํ URL์ ๋ก๊ทธ์ธ ๊ณผ์ ์ Selenium์ผ๋ก ์น ์กฐ์์ ํตํด ๋ก๊ทธ์ธ ๊ณผ์ ์ ์ฒ๋ฆฌํ๊ณ ,
        # ์น์์ ๋ณด์ฌ์ง๋ ์ ๋ณด๋ฅผ ํตํด ๋ก๊ทธ์ธ ์ฑ๊ณต/์คํจ ์ฌ๋ถ๋ฅผ ํ์ํฉ๋๋ค.
        # ... Login Process

        try:
            # ๋ก๊ทธ์ธ ์ฑ๊ณต
            self.ui.StatusLabel.setText("๋ก๊ทธ์ธ ์ฑ๊ณต")
        except:
          # Web์์ ๋ณด์ฌ์ง๋ ๋ก๊ทธ์ธ ์ฑ๊ณต/์คํจ ์ฌ๋ถ๋ฅผ ํ์ํ  ์ ์๋ Web ์ ๋ณด๋ฅผ ์ ๋, ๊ณต๋ฐฑ์ ์ ๊ฑฐํด String์ผ๋ก ๊ฐ์ ธ์จ๋ค.
          login_error = driver.find_element_by_xpath('Web_Xpath').text.strip()
          # ๋น๋ฐ๋ฒํธ๊ฐ ํ๋ฆฐ ๊ฒฝ์ฐ
          if "Password is not correct" in login_error:          
              self.ui.StatusLabel.setText("์์ด๋ ํน์ ํจ์ค์๋๋ฅผ ํ์ธํด์ฃผ์ธ์.")
          # ์ด๋ฏธ ๋ก๊ทธ์ธ ๋์ด ์๋ ๊ฒฝ์ฐ
          # ์ ๋ ๋ก๊ทธ์์ ํ, ์ฌ ๋ก๊ทธ์ธ์ ํ๋ ๋ฐฉ๋ฒ๋ฟ์ด๋ผ ํด๋น ๊ฒฝ์ฐ๋ฅผ ์ถ๊ฐํ์ต๋๋ค.
              elif "is already login" in login_error:
                # ... Login Process
                S_Password.send_keys(self.LoginPassword) # Password ๋ฐ์ดํฐ ์๋ ฅ
                S_Password.send_keys(Keys.RETURN) # Keys.RETURN == ์ํฐ

        # HTML5  <Select> ์ฒ๋ฆฌํ๊ธฐ
        # ๊ตฌ์กฐ
        # <select>
        #   <option>1</option>
        #   <option>2</option>
        # </select>
        GroupSelector = Select(driver.find_element_by_name('WEB_name'))
        # <Select>์ ์์๋ค์ options๋ฅผ ํตํด ๊ฐ์ ธ์ค๊ธฐ
        # ๋ ์์ธํ๋ Select์ options์ ๋ํด ๊ฒ์ํด๋ณด์๋ ๊ฒ์ ์ถ์ฒํฉ๋๋ค.
        GroupOptions = GroupSelector.options
        GroupList = list()
        eachList = list()

        # <Select>์ ์์๊ฐ์ ๊ธธ์ด๋งํผ ๋ฐ๋ณต๋ฌธ์ ํตํด List()์ ์ ์ฅํด ๋ฐ์ดํฐ๋ฅผ ๋ณด์ ํ  ์๊ฐ!
        for index in range(0, len(GroupOptions)):
            GroupList.append(GroupOptions[index].text)

        # ์ ๊ฐ ์ฒ๋ฆฌํ  Select ๋ฐ์ดํฐ๋ ์นดํ๊ณ ๋ฆฌ, ์นดํ๊ณ ๋ฆฌ ๋ณ ๋ฐ์ดํฐ
        # ์นดํ๊ณ ๋ฆฌ์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ง ๋ฆฌ์คํธ๋ฅผ ๋ฐ๋ณต
        for index in range(0, len(GroupList)):
            # ์นดํ๊ณ ๋ฆฌ ๋ด ๋ฐ์ดํฐ์ XPath์ ์ ๊ทผํ๊ธฐ ์ํด ๋ฌธ์์ด ์ฒ๋ฆฌ
            eachXpath = "//*[@id=\"itemGroupList." + GroupList[index] + "\"]"
            # ์นดํ๊ณ ๋ฆฌ ๋ด xPath ์ ๊ทผ
            eachElement = driver.find_element_by_xpath(eachXpath)
            # ์นดํ๊ณ ๋ฆฌ ๋ด ๋ฐ์ดํฐ๋ฅผ ์ ๊ทผํ๊ธฐ ์ํ HTML ํ๊ทธ ๊ตฌ์กฐ
            # <div id = "itemGroupList.GroupList[index]">
            #   <select>
            #     <option></option>
            #     <option></option>
            #   </select>
            # </div>
            # div ํ๊ทธ ๋ด, text ๋ด์ฉ ์ ๋ถ ๊ฐ์ ธ์ ํ์ธ ํ, tempList์ ํญ์ ํตํ ๋ฐ์ดํฐ ๊ตฌ๋ถ
            eachElementText = eachElement.get_attribute('textContent')
            tempList = eachElementText.split('\t')
            tempList = [x for x in tempList if x]
            # ์นดํ๊ณ ๋ฆฌ๋ณ ๋ฆฌ์คํธ์ ์ฝ์
            eachList.append(tempList)

        # ๋ฐ์ดํฐ๋ฅผ ๋ณด์ ํ๊ณ ์ ํ๋ txt ํ์ผ์ ์์์ ์ฒ๋ฆฌํ Data ์ฐ๊ธฐ
        # ๋ฐ์ดํฐ ๋ณ ๊ตฌ๋ถํ๊ธฐ ์ํด '#' ์ถ๊ฐ
        with open("./data_list.txt", "w") as dataList_file:
            for group in GroupList:
                if group != GroupList[-1]:
                    dataList_file.write(group + "#")
                else:
                    dataList_file.write(group)

            dataList_file.write("\n")

            for index in range(0, len(eachList)):
                for each in eachList[index]:
                    if each != eachList[index][-1]:
                        dataList_file.write(each + "#")
                    else:
                        dataList_file.write(each)

        self.ui.StatusLabel.setText("๋ฐ์ดํฐ๋ฅผ ์๋ฐ์ดํธ๊ฐ ์๋ฃ๋์์ต๋๋ค. ๋ฆฌํ๋ ์ ๋ฒํผ์ ๋๋ฌ์ฃผ์ธ์.\n๋ฐ์ดํฐ๋ฅผ ์ ํ ํ, ์ถ์ถ์ ๋๋ฌ์ฃผ์ธ์.")

    def RefreshFunc(self):
        # user_info.txt ํ์ผ ์กด์ฌ ์ฌ๋ถ ํ์
        if os.path.isfile("./user_info.txt"):
            with open("./user_info.txt", "r") as login_file:
                # readline์ ํตํด ์ต์์ ํ์ค๋ง ์ฝ์ด์ค๊ธฐ
                # ๋ก๊ทธ์ธ ์ํ(0, 1)
                StatusLabel = login_file.readline()
                # ๋ก๊ทธ์ธ ์ ๋ณด ์ผ์น
                if StatusLabel == "1\n":
                    self.ui.UpdateButton.setEnabled(True)
                # ๋ก๊ทธ์ธ ์ ๋ณด ํ๋ฆผ
                else:
                    self.ui.UpdateButton.setEnabled(False)

        # ๊ธฐ์กด ์นดํ๊ณ ๋ฆฌ์ ์๋ ๋ฐ์ดํฐ ์ญ์ 
        self.ui.GroupBox.clear()
        # data_list ํ์ผ ์กด์ฌ ์ฌ๋ถ ํ์ํด ์์ ๊ฒฝ์ฐ ์๋ฐ์ดํธ
        if os.path.isfile("./data_list.txt"):
            with open("./data_list.txt", "r") as dataList_file:
                try:
                    # ์นดํ๊ณ ๋ฆฌ๋ฅผ ํ ์ค์์์ #์ผ๋ก ๊ตฌ๋ถํด ์ฒ๋ฆฌํ๊ธฐ ๋๋ฌธ์ ์๋์ ๊ฐ์ด ์ฒ๋ฆฌ
                    ComboBoxData = dataList_file.readline()
                    ComboBoxData = ComboBoxData.strip()
                    ComboBoxList = ComboBoxData.split("#")
                    # ๋ฆฌ์คํธ ๋ฐ์ดํฐ๋ฅผ QComboBox์ ์ถ๊ฐ
                    for item in ComboBoxList:
                        self.ui.GroupBox.addItem(item)
                except:
                    return

        # ๊ธฐ์กด ์นดํ๊ณ ๋ฆฌ๋ณ ๋ฐ์ดํฐ ์ญ์ 
        self.ui.GroupListWidget.clear()
        # data_list ํ์ผ ์กด์ฌ ์ฌ๋ถ ํ์ํด ์์ ๊ฒฝ์ฐ ์๋ฐ์ดํธ
        if os.path.isfile("./data_list.txt"):
            with open("./data_list.txt", "r") as dataList_file:
                try:
                    # ๋ฐ์ดํฐ ๋งจ ์ฒซ ์ค์ ์นดํ๊ณ ๋ฆฌ ์ด๋ฏ๋ก, 2๋ฒ์งธ ๋ถํฐ ์ฝ์ ์ ์๋๋ก ์๋์ ๊ฐ์ด ์ฒ๋ฆฌ
                    data = dataList_file.readlines()[1]
                    # ์ค๊ฐ์ ์๊ธธ ์ ์๋ ๊ณต๋ฐฑ ์ ๊ฑฐ
                    data = data.strip()
                    dataList = data.split("#")
                    for dafaultItem in dataList:
                        self.ui.GroupListWidget.addItem(dafaultItem)
                except:
                    return

    def dataExtractFunc(self):
      # ์ถํ ์ค๋ช...
```
์ ๊ณผ์ ์ ์งํํ๋ฉด์, ๊ฐ์ฅ ๋ณต์กํ๋ ๋ถ๋ถ์ HTML5์ Select ๋ด ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ค๋ ๋ถ๋ถ์ด์๋ค.  

์นดํ๊ณ ๋ฆฌ๋ Select ๋ด๋ก ์ ๊ทผ์ ํ  ์ ์์๋๋ฐ, ์นดํ๊ณ ๋ฆฌ๋ณ ๋ฐ์ดํฐ์ Select์ ์นดํ๊ณ ๋ฆฌ ๋ฆฌ์คํธ๋ฅผ ํตํด ์ ๊ทผํ๊ธฐ ์ํด์๋ div์ ์ ๊ทผํด์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์์ผ ํ๋๋ฐ div ์ Select์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ค๋ ๋ฐฉ์์ ์ ์ ๋ฆฌ๋์ด ์์ง ์๊ธฐ ๋๋ฌธ์ด๋ค.  

๊ทธ๋์, `div์ ์ ๊ทผํด์ text์ ๋ชจ๋  ๋ด์ฉ์ ๊ฐ์ ธ์ ํด๋น ๋ฐ์ดํฐ๋ฅผ ์ฒ๋ฆฌํ๋ ๊ฒ์ ์ด๋จ๊น` ๋ผ๊ณ  ์๊ฐํ๋ค. ๋ํ, ํ๋ก๊ทธ๋จ ๋ด์์ ๋๋ฒ๊น ํน์ print๋ฅผ ์ฐ์ด๋ณด๋ฉด์ ๋ฐ์ดํฐ๋ฅผ ํ์ธํด๊ฐ๋ฉฐ ํ๋ ๊ฒ์ ์ถ์ฒํ๋ค.  

์ง๊ธ ์๊ฐํด๋ณด๋ฉด ๋ฆฌ์คํธ๋ฅผ 2๊ฐ ์ด์ฉํด์ ๋ฐ๋ณต์ ์ผ๋ก ์์ํ๋ ๊ฒ์ ์ถํ์ ๋ฌธ์ ๊ฐ ์๊ธฐ๊ฑฐ๋ ๋ณ๊ฒฝํด์ผํ  ๊ฒฝ์ฐ, ๋ฐ์ดํฐ๋ฅผ ํ์ธํ๋๋ฐ ๋ฒ๊ฑฐ๋ก์์ด ์์ ๊ฒ ๊ฐ๋ค. ์๋ฃ๊ตฌ์กฐ๋ฅผ Dict์ผ๋ก ์ ์ธํด ์นดํ๊ณ ๋ฆฌ ๋ณ ๋ฆฌ์คํธ๋ฅผ ๋ง๋๋๊ฒ ๋ ์ข์ ๊ฒ ๊ฐ๋ค.  

## PyQt5 ์ด๋ฏธ์ง ์ถ๊ฐ  

MainWindow UI์์ ํ๋จ์ ์ด๋ฏธ์ง๋ฅผ ์ถ๊ฐํ๊ธฐ ์ํด ์๋์ ๊ฐ์ด ์์ฑํ์์ต๋๋ค.  

QLabel์์ setPixmapํจ์๋ฅผ ํตํด์ ์ด๋ฏธ์ง๋ฅผ ์ง์ ํ  ์ ์์ผ๋ฏ๋ก, ์๋์ ๊ฐ์ ๋ฐฉ์์ผ๋ก ์ด๋ฏธ์ง๋ฅผ ์ถ๊ฐํ  ์ ์์ต๋๋ค.  

```python
# PyQt5 ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ถ๊ฐ๊ฐ ํ์ํฉ๋๋ค.
BASE_DIR = os.path.dirname(os.path.abspath(__file__))

class MainWindow(QtWidgets.QDialog):
    def __init__(self):
        self.ui = uic.loadUi(BASE_DIR + r'\DataSelect.ui')
        # ....

        qPixmapVar = QPixmap()
        qPixmapVar.load(BASE_DIR + r'\IMAGE_NAME.gif')
        self.ui.CompanyLogoLabel.setPixmap(qPixmapVar)
```  

## PyQt5 QLabel ์ฌ์ฉ ๋ฐฉ๋ฒ  

`QLabel`์ ์ฃผ์ ํจ์  
- .text() : Label์ ์ฐ์ฌ์๋ ๊ธ์๋ฅผ ๊ฐ์ ธ์จ๋ค.  
- .setText(String) : Label์ ์๋กญ๊ฒ ๊ธ์๋ฅผ ์์ฑ, Parameter์๋ Label์ ํ์ํ  ๊ธ์๊ฐ ์๋ ฅ  
- .clear() : Label์ ์ฐ์ฌ์๋ ๊ธ์๋ฅผ ์ง์ด๋ค.  

> ํ๋ก์ ํธ ๋ด ์ฌ์ฉ ์์  

```python
version = "Version : 1.0"

class MainWindow(QtWidgets.QDialog):
    def __init__(self):
        self.ui.VersionLabel.setText(version)
```



---
**๐ข๊ฐ๋ฐ ์๋ฃ ๋ ํ๋ก๊ทธ๋จ์ ๋ํ ๊ฐ๋ ๋ฐ ๋ด์ฉ์ ์ ๋ฆฌํ๊ณ ์ ํ๋ ๋ชฉ์ ์ผ๋ก ์์ฑ๋์์ต๋๋ค. ๊ถ๊ธํ์ ์ ์ ๋๊ธ ๋จ๊ฒจ์ฃผ์ธ์.๐ข**
{: .notice--primary}

๊ฐ์ฌํฉ๋๋ค.๐
