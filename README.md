# Flipkart Mobile Product Web Scraping

This project involves scraping mobile product data from Flipkart. The goal is to extract detailed information about mobile phones and store it for further analysis.

## Project Overview

The project scrapes various attributes of mobile products listed on Flipkart, including product names, ratings, number of reviews, descriptions, and prices. The data is then stored in an SQL database and converted into an Excel file for easy analysis.

## Features

- Scrape product names, ratings, number of reviews, descriptions, and prices.
- Store data in an SQL database using SQLite.
- Export the data to an Excel file.

## Requirements

- Python 3.x
- BeautifulSoup4
- Requests
- Pandas
- SQLAlchemy
- SQLite

## Setup

### 1. Install Required Libraries

To get started, install the required Python libraries. You can use `pip` to install them:

```bash
pip install beautifulsoup4 requests pandas sqlalchemy openpyxl
2. Clone the Repository
Clone this repository to your local machine:

bash
Copy code
git clone https://github.com/yourusername/flipkart-mobile-web-scraping.git
cd flipkart-mobile-web-scraping
3. Run the Scraping Script
The main script for scraping data is scrape_flipkart.py. Execute the script to start scraping:

bash
Copy code
python scrape_flipkart.py
Code Explanation
Web Scraping
The scraping script extracts the following information from Flipkart's mobile product pages:

python
Copy code
import requests
from bs4 import BeautifulSoup
import pandas as pd
from sqlalchemy import create_engine

# Define headers to avoid being blocked
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36'
}

# Function to scrape data from a single page
def scrape_flipkart_page(url):
    response = requests.get(url, headers=headers)
    soup = BeautifulSoup(response.content, 'html.parser')

    # Extract product details
    products = []
    for item in soup.find_all('div', class_='_1AtVbE'):
        name = item.find('div', class_='KzDlHZ')
        name_text = name.get_text(strip=True) if name else "N/A"
        
        rating = item.find('div', class_='XQDdHH')
        rating_text = rating.get_text(strip=True) if rating else "N/A"
        
        nrars = item.find('span', class_='Wphh3N')
        nrars_text = nrars.get_text(strip=True) if nrars else "N/A"
        
        description = item.find('ul', class_='G4BRas')
        description_text = description.get_text(strip=True) if description else "N/A"
        
        price = item.find('div', class_='Nx9bqj _4b5DiR')
        price_text = price.get_text(strip=True) if price else "N/A"
        
        products.append({
            'Product Name': name_text,
            'Rating': rating_text,
            'Number of Reviews': nrars_text,
            'Description': description_text,
            'Price': price_text
        })

    return products

# URL of the Flipkart mobile page
url = 'https://www.flipkart.com/mobiles/~mobile-prices'

# Scrape data
data = scrape_flipkart_page(url)

# Convert to DataFrame
df = pd.DataFrame(data)
Export Data to SQL
The scraped data is stored in an SQLite database:

python
Copy code
# Create SQLAlchemy engine
engine = create_engine('sqlite:///flipkart_mobiles.db', echo=False)

# Export DataFrame to SQL
df.to_sql('mobiles', con=engine, if_exists='replace', index=False)

# Verify data in SQL
query = 'SELECT * FROM mobiles LIMIT 5;'
df_sql = pd.read_sql(query, con=engine)
df_sql.head()
Convert SQL Data to Excel
The data from SQL is exported to an Excel file:

python
Copy code
# Read data from SQL
df_sql = pd.read_sql('SELECT * FROM mobiles', con=engine)

# Export to Excel
excel_file = 'flipkart_mobiles.xlsx'
df_sql.to_excel(excel_file, index=False)

# Verify by loading the Excel file
df_excel = pd.read_excel(excel_file)
df_excel.head()
Usage
Run the Scraper: Execute the scrape_flipkart.py script to start scraping data from Flipkart.
Check Data: The scraped data will be saved to flipkart_mobiles.db and flipkart_mobiles.xlsx.
License
This project is licensed under the MIT License - see the LICENSE file for details.

Acknowledgements
Flipkart for providing the data.
The developers of BeautifulSoup, Requests, Pandas, and SQLAlchemy for their excellent libraries.
css
Copy code

Feel free to modify the repository link, license, or any other details according to your projec
