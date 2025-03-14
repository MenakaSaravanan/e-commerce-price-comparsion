
import requests
from bs4 import BeautifulSoup

# Function to get the Amazon price of a product
def get_amazon_price(product):
    search_url = f"https://www.amazon.com/s?k={product.replace(' ', '+')}"
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3"}
    response = requests.get(search_url, headers=headers)

    if response.status_code != 200:
        return None
    
    soup = BeautifulSoup(response.content, 'html.parser')
    try:
        price = soup.find('span', {'class': 'a-price-whole'}).get_text()
        return price
    except AttributeError:
        return None

# Function to get the eBay price of a product
def get_ebay_price(product):
    search_url = f"https://www.ebay.com/sch/i.html?_nkw={product.replace(' ', '+')}"
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3"}
    response = requests.get(search_url, headers=headers)

    if response.status_code != 200:
        return None

    soup = BeautifulSoup(response.content, 'html.parser')
    try:
        price = soup.find('span', {'class': 's-item__price'}).get_text()
        return price
    except AttributeError:
        return None

# Main function to compare prices
def compare_prices(product):
    amazon_price = get_amazon_price(product)
    ebay_price = get_ebay_price(product)

    print(f"Comparing prices for: {product}")
    if amazon_price:
        print(f"Amazon Price: ${amazon_price}")
    else:
        print("Amazon Price: Not found")

    if ebay_price:
        print(f"eBay Price: {ebay_price}")
    else:
        print("eBay Price: Not found")

# Example usage
if __name__ == "__main__":
    product_name = "laptop"
    compare_prices(product_name)
