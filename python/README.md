```python
import requests,parsel,re,os
from tqdm import*
os.system("md video")
def a_aa(a):
    m=re.compile(r"[\/\\\:\*\?\"\<\>\|]")
    n=re.sub(m,"_",a)
    return n
url = 'https://haokan.baidu.com/web/author/listall?app_id=1707890966773713&ctime=16314328568539&rn=10&searchAfter=&_api=1'
headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36'
}
response = requests.get(url=url, headers=headers)
json_data = response.json()
videos = json_data['data']['results']
for i in tqdm(videos):
    play_url = 'https://haokan.baidu.com/v?vid='+i['content']['vid']
    html_data = requests.get(url=play_url, headers=headers)
    html_data_2 = parsel.Selector(html_data.text)
    href=html_data_2.css('#mse video::attr(src)').get()
    video_content = requests.get(url=href, headers=headers).content
    title=html_data_2.css('.videoinfo-title::text').get()
    with open("video\\"+a_aa(title)+'.mp4',mode="wb",encoding="utf-8") as f:
        f.write(href)
print()
print("爬取成功！")
```
