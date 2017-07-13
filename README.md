from bs4 import BeautifulSoup
import requests
import time
headers = {
    'User-Agent':'Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Mobile Safari/537.36'
}
urls = ['http://www.xunyingwang.com/movie/?rating=10&page={}'.format(str(i)) for i in range(1,7) ]

def get_myneed(url,data=None):
    time.sleep(2)
    wb_data = requests.get(url)
    mb_data = requests.get(url,headers=headers)
    soup = BeautifulSoup(wb_data.text, 'lxml')
    msoup = BeautifulSoup(mb_data.text,'lxml')

    movies = soup.select('div.meta > h1 > a')
    imgs = msoup.select('body > ul > li > div > a > img ')
    labels = soup.select('div.otherinfo')
    rates = soup.select('div.meta > h1 > em')
    for movie,img,label,rate in zip(movies,imgs,labels,rates) :
        data = {
        'movie' : movie.get_text(),
        'img' : img.get('src'),
        'label' : list(label.stripped_strings),
        'rate' : rate.get_text()
        }
        print(data)

for single_url in urls:
    get_myneed(single_url)
