from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from webdriver_manager.chrome import ChromeDriverManager
from fake_useragent import UserAgent
from bs4 import BeautifulSoup
import time


# Function to set up the WebDriver
def setup_driver():
    options = Options()   
    options.headless = True
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")
    options.add_argument("--disable-gpu")
    options.add_argument("--disable-blink-features=AutomationControlled")

    ua = UserAgent()
    random_user_agent = ua.random
    options.add_argument(f"user-agent={random_user_agent}")

    service = Service(ChromeDriverManager().install())
    driver = webdriver.Chrome(service=service, options=options)
    driver.maximize_window()

    driver.execute_script("Object.defineProperty(navigator, 'webdriver', {get: () => undefined});")
    driver.execute_script("window.navigator.chrome = {runtime: {}};")
    driver.execute_script("Object.defineProperty(navigator, 'languages', {get: () => ['en-US', 'en']});")
    driver.execute_script("Object.defineProperty(navigator, 'plugins', {get: () => [1, 2, 3, 4, 5]});")

    return driver

# Function to perform LinkedIn login and search
def linkedin_login_and_search(driver, email, password, company_name):
    try:
        driver.get("https://linkedin.com/uas/login")

        time.sleep(2)

        # Enter login credentials
        username = driver.find_element(By.ID, "username")
        username.send_keys(email)
        pword = driver.find_element(By.ID, "password")
        pword.send_keys(password)

        driver.find_element(By.XPATH, "//button[@type='submit']").click()
        time.sleep(50)

        # Search for the company
        search_box = driver.find_element(By.XPATH, "//input[@placeholder='Search']")
        search_box.send_keys(company_name)
        search_box.send_keys(Keys.RETURN)
        time.sleep(5)

        # Filter by companies
        driver.find_element(By.XPATH, "//button[text()='Companies']").click()
        time.sleep(5)

        # Extract company information
        page_source = driver.page_source
        soup = BeautifulSoup(page_source, 'html.parser')

        company = soup.find('span', {'class': 'entity-result__title-text t-16'})
        if company:
            print("Company Name: ", company.text.strip())

        company_domain_text = soup.find('div', {'class': 'entity-result__primary-subtitle t-14 t-black t-normal'})
        if company_domain_text:
            company_domain = company_domain_text.text.split('•')[0].strip()
            print("Company Subtitle: ", company_domain)

    except Exception as e:
        print(f"Exception occurred: {e}")
    finally:
        driver.quit()

# Main execution
if __name__ == "__main__":
    email = ""
    password = ""
    company_name = ""

    driver = setup_driver()  
    linkedin_login_and_search(driver, email, password, company_name)  
