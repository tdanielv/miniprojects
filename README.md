'''Parsing site'''

    from bs4 import BeautifulSoup
    import requests

    def save():
        with open('parse_info.txt', 'a') as file:
            file.write(f"{comp['title']}, '---', {comp['price']}, '---', {comp['link']}", end='\n')

    def parse():
        URL = 'https://www.kufar.by/'
        HEADERS = {
            'User-Agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 10_0_2 like Mac OS X) AppleWebKit/602.1.50 (KHTML, like Gecko) Version/10.0 Mobile/14A456 Safari/602.1'
        }
        response = requests.get(URL, headers= HEADERS)
        soup = BeautifulSoup(response.content, 'html.parser')
        items = soup.find_all('a', class_ = 'kf-OQl-42d1b')
        comps = []

        for item in items:
            comps.append({
                'title': item.find('h3', class_ = 'kf-OQhk-52f8b').get_text(),
                'price': item.find('p', class_ = 'kf-OQxz-1c8eb').get_text(),
                'link': item.get('href')
            })
            global comp
            for comp in comps:
                if '' in comp['title']:
                    print(f"{comp['title']}, '---', {comp['price']}, '---', {comp['link']}", end='\n')
                    save()
    parse()
