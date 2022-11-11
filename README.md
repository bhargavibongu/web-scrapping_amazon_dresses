# web-scrapping_amazon_dresses
import pandas as pd
import time
from selenium import webdriver as wb
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import NoSuchElementException
from selenium.common.exceptions import StaleElementReferenceException
import math
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.remote.webelement import WebElement
import numpy as np

driver = wb.Chrome(ChromeDriverManager().install())
driver.maximize_window()


search_urls=[]
for num in range(1,401):
    search_urls.append("https://www.amazon.in/s?i=apparel&rh=n%3A1571271031%2Cn%3A1571272031%2Cn%3A1953602031%2Cn%3A1968253031%2Cn%3A1968255031&s=apparels&page="+str(num))


brand = []
desc = []
disc_price = []
actual_price = []
image_url = []
product_url = []
for search_url in search_urls:
   
    print('search_url: ',search_url)

    driver.get(search_url);
   

    search_results = driver.find_elements_by_xpath('//div[contains(@data-component-type,"s-search-result")]')
   
    for search_result in search_results:
       
        try:
            brand_text = search_result.find_element_by_css_selector('.s-line-clamp-1').text
            if brand_text.strip() =='':
                brand.append(np.nan)
            else:
                brand.append(brand_text)
        except:
            brand.append(np.nan)
           
        try:
            desc_text = search_result.find_element_by_css_selector('.a-size-base-plus.a-color-base.a-text-normal').text
            if desc_text.strip() =='':
                desc.append(np.nan)
            else:
                desc.append(desc_text)
        except:
            desc.append(np.nan)

        try:
            disc_price_text = search_result.find_element_by_css_selector('.a-price-whole').text
            if disc_price_text.strip() =='':
                disc_price.append(np.nan)
            else:
                disc_price.append(disc_price_text)
        except:
            disc_price.append(np.nan)

        try:
            actual_price_text = search_result.find_element_by_css_selector('.a-price.a-text-price').text.split('₹')[1]
            if actual_price_text.strip() =='':
                actual_price.append(np.nan)
            else:
                actual_price.append(actual_price_text)
        except:
            actual_price.append(np.nan)

        try:
            image_url_text = search_result.find_element_by_css_selector('.s-image').get_attribute('src')
            if image_url_text.strip() =='':
                image_url.append(np.nan)
            else:
                image_url.append(image_url_text)
        except:
            image_url.append(np.nan)

        try:
            product_url_text = search_result.find_element_by_css_selector('.a-link-normal.a-text-normal').get_attribute('href')
            if product_url_text.strip() =='':
                product_url.append(np.nan)
            else:
                product_url.append(product_url_text)
        except:
            product_url.append(np.nan)
    print(len(brand),len(desc),len(disc_price ),len(actual_price ),len(image_url ),len(product_url))
