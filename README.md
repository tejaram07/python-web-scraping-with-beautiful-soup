from bs4 import BeautifulSoup
import requests, openpyxl

excel = openpyxl.Workbook()
sheet = excel.active
sheet.title = 'Top Rated Movies'
sheet.append(['Movie Rank','movie Name'])

try:
    source = requests.get('https://www.imdb.com/list/ls091520106/')
    source.raise_for_status()
    soup = BeautifulSoup(source.text,'html.parser')
    movies = soup.find('div',class_="lister-list").find_all('div',class_="lister-item mode-detail")
    for movie in movies :
        name = movie.find('h3',class_="lister-item-header").a.text
        rank = movie.find('h3',class_="lister-item-header").get_text(strip=True).split('.')[0]
        print(rank,name)
        sheet.append([rank,name])

except Exception as e:
    print(e)

excel.save('Imdb Top Rated movies.xlsx')
