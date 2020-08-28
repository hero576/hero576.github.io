title: Selenium+Chrome Headless
author: hero576
date: 2018-06-12 22:29:11
tags:
---
## 爬取优酷

- 重写downloadmiddleware，用selenium返回渲染页面
- 配置cookie，否则视频无法浏览
```python
from scrapy import signals
from scrapy.http import HtmlResponse
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

class PhantomJSMiddleware(object):
    chrome_options = Options()
    chrome_options.add_argument('--headless')
    chrome_options.add_argument('--disable-gpu')
    chrome_options.add_argument('--disable-gpu')
    cookie = {
        'P_F': '1', 'P_T': '1528813933', '__ysuid': '1528806696289ph5', '__ayft': '1528806696292',
        '__aysid': '1528806696292mV9', '__ayscnt': '1', 'cna': 'KKumE5+gl1sCAd9HVQmKmius', 'juid': '01cfpvlq5rv5h',
        'seid': '01cfpvlq5sqpg', 'referhost': '', '_m_h5_tk': 'ed30ebe6e53a22f1d47aa4e71f79ec87_1528811377376',
        '_m_h5_tk_enc': 'f405a8b0d3a3915c970f07e4f119b833', 'yseid': '1528806699419zrjY1e', 'yseidcount': '1',
        'ycid': '0', '_uab_collina': '152880670018967700378597',
        '_umdata': '2BA477700510A7DF0099EB2C8D59E1BB58600131A08A093B3209A391A58152A56DBC59A3C3DA6766CD43AD3E795C914CDBEF0B2B1C6A45E26439C70995D62189',
        'P_pck_rm': 'KzZ%2B7BM%2BJxFUBmquoAbZgYhih1oPzIYnc4rH1ncdmxYCNrVam93Pp31JS2PZ9ZBDwr3CZyFbnEbTFU9OoEEnx9VsA2jJGxI24vuiPhYKKp9Bjn%2FlZyMuWq3Yx1nf84NpO5kN%2FAtXW1VibTnfxgGXZ4pV5mplSGFzLZpIk4JxBFQ%3D',
        'P_j_scl': 'hasCheckLogin', 'P_gck': 'NA%7C3bTVpk%2BE0zRfemcPASmqYQ%3D%3D%7CNA%7C1528806726826',
        '__arpvid': '1528806730297glhTlS-1528806730313', '__arycid': 'dz-3-00', '__arcms': 'dz-3-00', '__aypstp': '2',
        '__ayspstp': '2', 'seidtimeout': '1528808530657', 'ypvid': '1528806733188nQW7dA', 'ysestep': '2',
        'yseidtimeout': '1528813933190', 'ystep': '2',
        'P_sck': 'x6256zAi1jpqUvMox1qy6O1K9PXUhSkxe6FSNNPapUK2eNVng1A/fvUGcfmxj00+CO3PhyJ+0moRIF8IK4kNzQb25qltr8csNWnAGgXEBmOSRHcec5YrPT1ViBlqSnobqlRpU05KIoAUAeEui/8a2Wm3a/weYK5dPPIAc5BTT4A=',
        'P_sck.sig': 'AK5oRg1h9aX-fq_b7ELRGi4oyfVA2DPiZwLpzGCPZVY', '__ayvstp': '4', '__aysvstp': '4',
        'isg': 'BMbGrFIA4c6xlbUqXXNeDjhhF7yIjwuIcIyefrDvsunEs2bNGLda8axBj-5_GwL5',
    }

    @classmethod
    def process_request(cls, request, spider):


        if 'PhantomJS' in request.meta:
            # driver = webdriver.Chrome(chrome_options=cls.chrome_options)
            driver = webdriver.Chrome()

            driver.get(request.url)

            for name, value in cls.cookie.items():
                cookie_ = {
                    'domain': '.youku.com',
                    'name': name,
                    'value': value,
                    'expires': '',
                    'path': '/',
                    'httpOnly': False,
                    'HostOnly': False,
                    'Secure': False,
                }
                driver.add_cookie(cookie_)

            # driver.add_cookie(request.headers)
            print('***********', request.url)
            print(request.meta)
            # if request.meta.get('data',0):
            #     print('----------------',request.url)
            #     wait=WebDriverWait(driver,20)
            #     wait.until(EC.presence_of_element_located((By.TAG_NAME,'video')))
            driver.get(request.url)
            content = driver.page_source.encode('utf-8')
            driver.quit()
            return HtmlResponse(request.url, encoding='utf-8', body=content, request=request)

```