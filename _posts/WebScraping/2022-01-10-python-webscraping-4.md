---
title: "[WebScraping] PyQt5 GUI(QComboBox, QListWidget, QDateEdit) 사용 방법"

categories:
  - WebScraping
tags:
  - [Crawler, Python, Selenium, PyQt5, Qt Designer]

toc:  true
toc_sticky: true

date: 2022-01-10
last_modified_at: 2022-01-10
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

## PyQt5 QComboBox 사용 방법  

`QComboBox`의 주요 함수  
- 항목의 반환  
-- .currentIndex() : 현재 ComboBox에서 선택된 항목의 index를 반환  
-- .currentText() : 현재 ComboBox에서 선택된 항목의 글자를 반환  
-- .count() : ComboBox에 몇개의 항목이 있는지를 그 개수를 반환  
-- .itemText(index) : Index번째에 어떤 항목이 있는지 그 글자를 반환, Parameter로 찾을 항목의 Index를 입력  
- 항목의 추가 및 삭제  
-- .addItem(String) : ComboBox의 맨 뒤에 항목을 추가, Parameter로 추가할 항목의 글자를 입력  
-- .insertItem(Index, String) : Index번째에 String이라는 항목을 추가, Parameter로 항목을 추가할 위치(Index)와 글자를 입력  
-- .removeItem(Index) : ndex번째의 항목을 삭제, arameter로 삭제할 항목의 Index를 입력  
-- .clear() : ComboBox의 모든 항목을 삭제  

> 프로젝트 내 사용 예시  

![image](https://user-images.githubusercontent.com/37467408/148718475-bc7b2d20-da43-4ac2-b510-cdd991509cd8.png)  

```python
# 예시 1 : QComboBox에 데이터를 넣어둔 후, 해당 인덱스를 입력해 접근하는 방법
QComboBox1.setCurrentIndex(23)
QComboBox2.setCurrentIndex(5)

# 예시 2 : txt 파일을 읽어 해당 데이터를 리스트로 만들어 QComboBox에 추가하는 방법
with open("./data_list.txt", "r") as dataList_file:
    ComboBoxData = dataList_file.readline()
    ComboBoxData = ComboBoxData.strip()
    ComboBoxList = ComboBoxData.split("#")
    for item in CoboBoxList:
        QComboBox.addItem(item)
```  

## PyQt5 QListWidget 사용 방법  

`QListWidget`의 시그널  
- QListWidget.itemClicked.connect(함수) : QListWidget의 항목이 클릭되었을 때 기능 실행  
- QListWidget.itemDoubleClicked.connect(함수) : QListWidget의 항목이 더블클릭되었을 때 기능 실행  
- QListWidget.currentItemChanged.connect(함수) : QListWidget에서 다른 항목이 선택되었을 때  

`QListWidget`의 함수  
- 항목의 반환  
-- currentRow() : ListWidget에서 선택한 항목이 몇번째 항목인지를 반환  
-- currentItem() : ListWidget에서 선택한 항목을 반환, 반환된 값은 QListWidgetItem의 객체  
-- item(row) : ListWidget에서 Row번째 항목을 반환, 반환된 값은 QListWidgetItem의 객체  
- 항목의 추가, 삭제  
-- addItem(String) : ListWidget에 새로운 항목을 추가, Parameter로 추가할 항목의 문자열을 입력
-- insertItem(row, String) : ListWiget의 row번째에 새로운 항목을 추가, Parameter로 추가할 위치와 추가할 항목의 문자열을 입력  
-- takeItem(row) : ListWidget의 row번째 항목을 삭제, 삭제된 항목은 QListWidgetItem의 형태로 반환  
-- clear() : ListWidget의 모든 항목을 삭제  

> 프로젝트 내 사용 예시  

![image](https://user-images.githubusercontent.com/37467408/148718980-10ad6bce-056a-4914-a6d9-da2e16c70058.png)  

```python
# 예제 1 : QListWidget에서 데이터 추가/삭제
self.ui.QListWidget1.itemDoubleClicked.connect(self.SelectDataFunc)
self.ui.QListWidget2.itemDoubleClicked.connect(self.MinusDataFunc)

def SelectDataFunc(self):
    selectItem = self.ui.QListWidget1.currentItem().text()
    self.ui.QListWidget2.addItem(selectItem)

def MinusDataFunc(self):
    minusitemRow = self.ui.QListWidget2.currentRow()
    self.ui.QListWidget2.takeItem(minusitemRow)

# 예제 2 : txt내용 기준으로 QListWidget 데이터 업데이트 기능
self.ui.QListWidget1.clear()
if os.path.isfile("./data_list.txt"):
    with open("./data_list.txt", "r") as dataList_file:
        try:
            data = dataList_file.readlines()[1]
            data = data.strip()
            dataList = data.split("#")
            for dafaultItem in dataList:
                self.ui.QListWidget1.addItem(dafaultItem)
        except:
            return

# 예제 3 : QComboBox내용에 따라 QListWidget 내용 바꾸기
self.ui.GroupBox.activated[str].connect(self.onChangeList)

def onChangeList(self):
    groupBoxIndex = self.ui.GroupBox.currentIndex()
    self.ui.GroupListWidget.clear()
    if os.path.isfile("./data_list.txt"):
        with open("./data_list.txt", "r") as dataList_file:
            try:
                data = dataList_file.readlines()[groupBoxIndex + 1]
                data = data.strip()
                dataList = data.split("#")
                for item in dataList:
                    self.ui.GroupListWidget.addItem(item)
            except:
                return
```

## PyQt5 QDateEdit 사용 방법  

`QDateEdit`의 함수  
- 현재 상태의 반환  
-- .date() : QDateEdit의 날짜값을 반환, QDate형태의 객체가 반환  
-- .maximumDate() : QDateEdit의 날짜값의 최댓값을 반환, QDate형태의 객체가 반환  
-- .minimumDate() : QDateEdit의 날짜값의 최솟값을 반환, QDate형태의 객체가 반환  

- QDateEdit의 값과 입력/표시형식  
-- .setDate(QDate) : QDateEdit의 날짜값을 반환, Parameter로 새로 설정할 날짜값을 가진 QDate 객체가 필요  
-- .calendarPopup() : QDateEdit의 CalendarPopup이 속성이 참인지, 거짓인지를 반환  
-- .setCalendarPopup(bool) : QDateEdit의 CalendarPopup 속성을 변경할 수 있는 함수, Parameter로 True/False 값  
-- .setDisplayFormat(Format Text) : QDateEdit이 어떤 형식으로 날짜와 시간을 보여줄지를 설정, Parameter로 형식문자  

- QDateEdit의 최대/최소에 관련된 함수  
-- .setDateRange(min, max) : QDateEdit의 날짜 범위를 설정, Parameter로 날짜의 최솟값, 최댓값을 입력  
-- .setMaximumDate(max) : QDateEdit의 날짜값의 최댓값을 설정, Parameter로 새로운 날짜의 최댓값을 입력  
-- .setMinimumDate(min) : QDateEdit의 날짜값의 최솟값을 설정, Parameter로 새로운 날짜의 최솟값을 입력  
-- .clearMaximumDate() : QDateEdit의 날짜값의 최댓값을 기본값으로 변경  
-- .clearMinimumDate() : QDateEdit의 날짜값의 최솟값을 기본값으로 변경  

> 프로젝트 내 사용 예시  

![image](https://user-images.githubusercontent.com/37467408/148719707-562c75b2-c823-41d8-bd86-59b095ffb501.png)  

```python
# 예제 1 : 오늘 날짜를 기준으로 시간 설정하기
import datetime

currentTime = str(datetime.datetime.today().date()).replace('-', '/')
self.ui.QDateEdit1.setDate(QDate.fromString(currentTime, "yyyy/MM/dd"))
self.ui.QDateEdit2.setDate(QDate.fromString(currentTime, "yyyy/MM/dd"))

# 예제 2 : 데이터 가져와 이용하기
FromDate = self.ui.QDateEdit1.date()
FromYear, FromMonth, FromDay = FromDate.toString("yyyy"), FromDate.toString("MM"), FromDate.toString("dd")
```

---
**🐢개발 완료 된 프로그램에 대한 개념 및 내용을 정리하고자 하는 목적으로 작성되었습니다. 궁금하신점은 댓글 남겨주세요.🐢**
{: .notice--primary}

감사합니다.😊
