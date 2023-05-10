---
title: "Python selenium 모듈을 이용한 웹크롤링"
excerpt: "OncoKB 사이트에서 원하는 data를 추출하기 위한 웹크롤러"

categories:
  - Python
tags:
  - datahandling
last_modified_at: 2021-10-03
---

Extracting interest data from websites using Selenium on python

## **1. Install selenium** ##
```shell-script
pip install selenium
```

## **2. Run debugger chrome** ##

```python
import subprocess
subprocess.Popen(r'C:\Program Files\Google\Chrome\Application\chrome.exe --remote-debugging-port=9222 --user-data-dir="C:\chrometemp"')
```


## **3. Run webdriver** ##
### 3.0. Maually download `chromdriver.exe` ###
Download driver on [release page](https://chromedriver.chromium.org/downloads)

### 3.1. Webdriver options ###
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

option = Options()
option.add_experimental_option("debuggerAddress", "127.0.0.1:9222")
option.add_argument('--no-sandbox')
option.add_argument("--user-agent=Mozilla/5.0 (iPhone; CPU iPhone OS 6_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/6.0 Mobile/10A5376e Safari/8536.25")
options.add_argument('--window-size=1920X1080')
option.add_argument('--disable-dev-shm-usage')
```

### 3.2. `chromedriver-autoinstaller` ###
#### 3.2.1 Installation ####
```shell-script
pip install chromedriver-autoinstaller
```
#### 3.2.2 Run `chromedriver-autoinstaller` ####
```python
from selenium import webdriver
import chromedriver_autoinstaller

chrome_ver = chromedriver_autoinstaller.get_chrome_version().split('.')[0]  #check version of chromedriver

try:
    driver = webdriver.Chrome(f'./{chrome_ver}/chromedriver.exe')   
except:
    chromedriver_autoinstaller.install(True)
    driver = webdriver.Chrome(f'./{chrome_ver}/chromedriver.exe')

driver.implicitly_wait(10)
```
## **4. Web scrapping** ##

```python
# 4.1. go to target website
driver.get('https://www.oncokb.org/gene/'+genes[i]) 

# 4.2. get element
elem = driver.find_elements(By.CLASS_NAME,'RRT__tab--first')

# 4.3. click element
elem[0].click()

# 4.4. get text from Web element
elem[0].text.split('\n')
```

## **5. Close driver** ##
```python
driver.close()
```

## **6. Remove cache** ##

```python
import shutil
try:
    shutil.rmtree(r"c:\chrometemp")
except FileNotFoundError:
    pass
```

