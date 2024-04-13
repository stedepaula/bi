Atividade business intelligence

import requests
from bs4 import BeautifulSoup
import csv

def extrair_precos(url):
    response = requests.get(url)
    if response.status_code == 200:
        soup = BeautifulSoup(response.text, 'html.parser')
        precos = []
        for produto in soup.find_all('div', class_='product-card'):
            nome = produto.find('a', class_='product-card__name').text.strip()
            preco = produto.find('span', class_='price__sales').text.strip()
            precos.append({'nome': nome, 'preco': preco})
        return precos
    else:
        print("Falha ao acessar a página:", response.status_code)
        return None

def main():
    url = 'https://www.zattini.com.br/'
    precos = extrair_precos(url)
    if precos:
        with open('precos_produtos_zattini.csv', 'w', newline='', encoding='utf-8') as csvfile:
            fieldnames = ['nome', 'preco']
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
            writer.writeheader()
            for produto in precos:
                writer.writerow(produto)
        print("Dados extraídos e salvos com sucesso em 'precos_produtos_zattini.csv'")
    else:
        print("Falha ao extrair dados.")

if __name__ == "__main__":
    main()
