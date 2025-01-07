
# Create an eBay Scraper in Python Using ScraperAPI

In this guide, we’ll build an eBay scraper using Python and ScraperAPI to fetch and parse eBay listings data. eBay, a popular online auction platform, is rich with listings that can be scraped to monitor prices, analyze trends, or gather data for further processing.

We will write two scripts:
1. **Fetch Listings**: To gather eBay listing URLs and save them into a file.
2. **Parse Listings**: To extract detailed information like title, price, seller, and images.

By using [ScraperAPI](https://bit.ly/Scraperapi), we can bypass common scraping challenges like IP bans and dynamic rendering, ensuring smooth and reliable data extraction.

---

## Why Use ScraperAPI?

ScraperAPI is a robust and affordable solution that simplifies web scraping by handling proxies, CAPTCHAs, and JavaScript rendering for you. Here’s why it’s the best choice for scraping eBay:

- **Bypass IP Blocks**: Automatically rotates proxies to avoid bans.
- **Headless Browser Support**: Handles dynamic content without additional setup.
- **Ease of Use**: A simple API to integrate with your scripts.

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Step 1: Fetch eBay Listing URLs

The first script fetches eBay listing URLs from a specific category and saves them to a text file.

### Python Script for Fetching URLs

```python
import requests
from bs4 import BeautifulSoup

if __name__ == '__main__':
    API_KEY = None
    links_file = 'links.txt'
    links = []

    # Load API key from file
    with open('API_KEY.txt', encoding='utf8') as f:
        API_KEY = f.read().strip()

    # URL to scrape
    URL_TO_SCRAPE = 'https://www.ebay.com/b/Computer-Components-Parts/175673/bn_1643095'

    # ScraperAPI parameters
    payload = {'api_key': API_KEY, 'url': URL_TO_SCRAPE, 'render': 'false'}

    # Fetch page content
    r = requests.get('http://api.scraperapi.com', params=payload, timeout=60)
    if r.status_code == 200:
        html = r.text.strip()
        soup = BeautifulSoup(html, 'lxml')

        # Extract listing links
        entries = soup.select('.s-item a')
        for entry in entries:
            if 'p/' in entry['href']:
                listing_url = entry['href'].replace('&amp;rt=nc#UserReviews', '')
                links.append(listing_url)

    # Save links to file
    if links:
        with open(links_file, 'a+', encoding='utf8') as f:
            f.write('\n'.join(links))
        print('Links stored successfully.')
```

This script uses ScraperAPI to fetch eBay listings and stores the URLs in a file named `links.txt`.

---

## Step 2: Parse eBay Listings

The second script parses individual listing pages and extracts details such as the title, price, seller, and image.

### Python Script for Parsing Listings

```python
import requests
from bs4 import BeautifulSoup

if __name__ == '__main__':
    record = {}
    price = title = seller = image = None

    # Load API key from file
    with open('API_KEY.txt', encoding='utf8') as f:
        API_KEY = f.read().strip()

    # URL to scrape
    URL_TO_SCRAPE = 'https://www.ebay.com/p/5034585650?iid=202781903791'

    # ScraperAPI parameters
    payload = {'api_key': API_KEY, 'url': URL_TO_SCRAPE, 'render': 'false'}

    # Fetch page content
    r = requests.get('http://api.scraperapi.com', params=payload, timeout=60)
    if r.status_code == 200:
        html = r.text
        soup = BeautifulSoup(html, 'lxml')

        # Extract title
        title_section = soup.select('.product-title')
        if title_section:
            title = title_section[0].text.strip()

        # Extract seller
        seller_section = soup.select('.seller-persona')
        if seller_section:
            seller = seller_section[0].text.replace('Sold by', '').replace('Positive feedbackContact seller', '').strip()

        # Extract price
        price_section = soup.select('.display-price')
        if price_section:
            price = price_section[0].text.strip()

        # Extract image
        image_section = soup.select('.vi-image-gallery__enlarge-link img')
        if image_section:
            image = image_section[0]['src']

        # Store data in a record
        record = {
            'title': title,
            'price': price,
            'seller': seller,
            'image': image,
        }

        print(record)
```

### Example Output

When you run the script, you’ll get an output like this:

```json
{
  "title": "AMD Ryzen 3 3200G - 3.6GHz Quad Core (YD3200C5FHBOX) Processor",
  "price": "$99.99",
  "seller": "best_buy (698388)97.2%",
  "image": "https://i.ebayimg.com/images/g/ss8AAOSwsbhdmy2e/s-l640.jpg"
}
```

---

## Enhancing Your Scraper

This script can be further customized for advanced use cases, such as:

- **Price Monitoring**: Track price changes for specific products.
- **Data Storage**: Save extracted data to a database or CSV file for further analysis.
- **Dynamic Categories**: Scrape multiple eBay categories dynamically by updating the input URLs.

---

## Conclusion

By using ScraperAPI and Python, scraping eBay becomes straightforward and hassle-free. This setup lets you extract valuable eBay data while bypassing common issues like IP bans and CAPTCHA blocks. ScraperAPI’s ease of use, combined with its robust capabilities, makes it an ideal solution for building reliable scrapers.

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)
