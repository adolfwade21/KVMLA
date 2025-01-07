
# How to Create an Amazon Scraper in Python Using ScraperAPI

Web scraping Amazon data is a popular use case for many developers and businesses, enabling tasks like price monitoring, product research, and data collection for analytics. In this guide, we'll build a Python scraper for Amazon using ScraperAPI to handle proxies, CAPTCHAs, and dynamic content rendering.

With ScraperAPI, you don’t have to worry about getting blocked or managing headless browsers—it handles everything for you.

---

## Overview of the Project

We’ll write two Python scripts for this project:

1. **`fetch.py`**: Fetches URLs of individual product listings from a specific Amazon category and saves them to a text file.
2. **`parse.py`**: Parses the details of a specific product URL and saves the data in JSON format.

We’ll use the following tools and libraries:
- **ScraperAPI** for seamless web scraping.
- **BeautifulSoup** for HTML parsing.
- **Requests** for HTTP requests.

---

## Why Use ScraperAPI?

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, allowing you to focus on collecting structured data from Amazon, Google, Walmart, and more.  

👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Script 1: Fetch Product Listing URLs

The first script, `fetch.py`, extracts product URLs from an Amazon category page and saves them to a file.

### Code: `fetch.py`

```python
import requests
from bs4 import BeautifulSoup

if __name__ == '__main__':
    headers = {
        'authority': 'www.amazon.com',
        'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36',
        'accept-language': 'en-US,en;q=0.9',
    }

    API_KEY = None
    links_file = 'links.txt'
    links = []

    with open('API_KEY.txt', encoding='utf8') as f:
        API_KEY = f.read().strip()

    URL_TO_SCRAPE = 'https://www.amazon.com/s?i=electronics&rh=n%3A172541%2Cp_n_feature_four_browse-bin%3A12097501011&lo=image'

    payload = {'api_key': API_KEY, 'url': URL_TO_SCRAPE, 'render': 'false'}

    r = requests.get('http://api.scraperapi.com', params=payload, timeout=60)

    if r.status_code == 200:
        soup = BeautifulSoup(r.text, 'lxml')
        links_section = soup.select('h2 > .a-link-normal')
        for link in links_section:
            url = 'https://amazon.com' + link['href']
            links.append(url)

    if links:
        with open(links_file, 'w', encoding='utf8') as f:
            f.write('\n'.join(links))

        print('Links stored successfully.')
```

### Explanation:

1. **Headers**: Customize headers to mimic a browser request.
2. **ScraperAPI**: API key and endpoint handle requests to Amazon.
3. **HTML Parsing**: Use BeautifulSoup to extract product links (`h2 > .a-link-normal`).
4. **Save Links**: Store extracted links in a `links.txt` file.

---

## Script 2: Parse Product Details

The second script, `parse.py`, takes a product URL, scrapes its details (title, price, availability, etc.), and saves the data as JSON.

### Code: `parse.py`

```python
import requests
from bs4 import BeautifulSoup

def parse(url):
    record = {}
    headers = {
        'authority': 'www.amazon.com',
        'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36',
        'accept-language': 'en-US,en;q=0.9',
    }

    payload = {'api_key': API_KEY, 'url': url, 'render': 'false'}

    r = requests.get('http://api.scraperapi.com', params=payload, timeout=60)

    if r.status_code == 200:
        soup = BeautifulSoup(r.text, 'lxml')
        title_section = soup.select('#productTitle')
        price_section = soup.select('#priceblock_ourprice')
        availability_section = soup.select('#availability')
        features_section = soup.select('#feature-bullets')
        asin_section = soup.find('link', {'rel': 'canonical'})

        title = title_section[0].text.strip() if title_section else None
        price = price_section[0].text.strip() if price_section else None
        availability = availability_section[0].text.strip() if availability_section else None
        features = features_section[0].text.strip() if features_section else None
        asin = asin_section['href'].split('/')[-1] if asin_section else None

        record = {
            'title': title,
            'price': price,
            'availability': availability,
            'asin': asin,
            'features': features
        }

    return record


if __name__ == '__main__':
    API_KEY = None

    with open('API_KEY.txt', encoding='utf8') as f:
        API_KEY = f.read().strip()

    result = parse('https://www.amazon.com/dp/B07B49QG1H/')
    print(result)
```

### Explanation:

1. **ScraperAPI**: Pass the product URL to ScraperAPI, which handles all proxy and CAPTCHA challenges.
2. **HTML Parsing**: Extract details like title, price, availability, and ASIN using BeautifulSoup.
3. **Output**: Return the scraped data as a dictionary.

---

## Use Cases and Further Enhancements

With these scripts, you can:
- Build an Amazon price monitoring tool.
- Create an ASIN scraper for product research.
- Extract Amazon product reviews.

To scale up, consider these optimizations:
- Implement multithreading to scrape multiple products simultaneously.
- Save scraped data directly to a database.
- Use ScraperAPI’s headless browser support for more complex scraping tasks.

---

## Conclusion

In this guide, you learned how to scrape Amazon data using Python and ScraperAPI. By combining ScraperAPI with tools like BeautifulSoup and Requests, you can build efficient scrapers that handle proxies, CAPTCHAs, and dynamic content effortlessly.

Ready to streamline your web scraping process?  

👉 [Start your free trial with ScraperAPI today!](https://bit.ly/Scraperapi)
