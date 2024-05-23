# BraveAI-Scraper

**BraveAI-Scraper** is a Python tool that scrapes data from Brave AI, allowing users to access model responses without needing an API key. This project offers a free solution for developers and researchers to integrate Brave AI's capabilities into their applications.

## Features

- Scrape data from Brave AI
- Access model responses without an API key
- Headless browsing option

## Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/yourusername/BraveAI-Scraper.git
   cd BraveAI-Scraper
   ```

2. Install the required dependencies:
   ```sh
   pip install playwright
   playwright install
   ```

## Usage

```python
import time
from playwright.sync_api import sync_playwright

class BraveSearcher:
    def __init__(self, headless=True):
        self.playwright = sync_playwright().start()
        self.browser = self.playwright.chromium.launch(headless=headless)
        self.page = self.browser.new_page(bypass_csp=True)

    def search(self, query):
        base_url = "https://search.brave.com"
        search_url = f"{base_url}/search?q={query}&source=llmSuggest&summary=1"
        start_time = time.time()  
        self.page.goto(search_url, timeout=100000)
        self.page.wait_for_selector('//div[@id="chatllm-context"]', timeout=30000)
        page_content = self.page.locator('//div[@id="chatllm-content"]').inner_text()
        end_time = time.time()  
        execution_time = end_time - start_time  
        print(f"Execution time: {execution_time} seconds") 
        return page_content

    def close(self):
        self.browser.close()
        self.playwright.stop()

if __name__ == '__main__':
    searcher = BraveSearcher(headless=True)
    try:
        query = "tell me a joke"
        result = searcher.search(query)
        print(result)
    finally:
        searcher.close()
```

## Example

To use the `BraveSearcher` class, initialize it, call the `search` method with your query, and finally close the browser instance:

```python
searcher = BraveSearcher(headless=True)
try:
    query = "tell me a joke"
    result = searcher.search(query)
    print(result)
finally:
    searcher.close()
```

## Disclaimer

This project is intended for educational purposes only. The developers are not responsible for any misuse of this tool. Please use it responsibly and in accordance with all applicable laws and regulations.

## License

This project is licensed under the MIT License.
