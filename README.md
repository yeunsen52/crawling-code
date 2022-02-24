######## crawling-code
######## image crawling in google using selenium
######## 가상환경 설정 python -m venv filename
######## 가상환경 실행 cd filename - cd Scripts - activate
######## pip install selenium==3.14.0
######## 가상환경 폴더 안에는 사진을 저장할 폴더,inclde,lib,Scripts,selenium,urllib3,chromedriver.exe
######## python.lnk,python amd,python.exe,google.py(파일생성),pyvenv.cfg 있어야 함
######## (가상환경)c://.../filename/python google.py

from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
from urllib.request import Request, urlopen

driver = webdriver.Chrome()
driver.get("https://www.google.co.kr/imghp?hl=ko&tab=ri&ogbl")
elem = driver.find_element_by_name("q")
elem.send_keys("오마이걸 지호")
elem.send_keys(Keys.RETURN)

SCROLL_PAUSE_TIME = 1

# Get scroll height
last_height = driver.execute_script("return document.body.scrollHeight")

while True:
    # Scroll down to bottom
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

    # Wait to load page
    time.sleep(SCROLL_PAUSE_TIME)

    # Calculate new scroll height and compare with last scroll height
    new_height = driver.execute_script("return document.body.scrollHeight")
    if new_height == last_height:
        try:
            driver.find_element_by_css_selector(".mye4qd").click()
        except:
            break
    last_height = new_height

images = driver.find_elements_by_css_selector(".rg_i.Q4LuWd")
count = 772
for image in images:
    try:
        image.click()
        time.sleep(2)
        imgUrl = driver.find_element_by_css_selector(".n3VNCb").get_attribute("src")
        f = open('여자호랑이상/'+str(count) + '.jpg','wb')
        req = Request(imgUrl, headers={'User-Agent': 'Mozilla/5.0'})
        data=urlopen(req).read()
        f.write(data)
        f.close()
        count = count + 1 
        if count == 1300 :
            break
    except:
        pass

driver.close()
