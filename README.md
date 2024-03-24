# Python-automation script
from selenium.webdriver.common.by import By
# Configure logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(message)s')
logger = logging.getLogger()

# Function to measure page load time
def measure_page_load_time(url):
    start_time = time.time()
    driver.get(url)
    end_time = time.time()
    return end_time - start_time

# Function to log to both console and file
def log(message):
    logger.info(message)
    with open('test_log.txt', 'a') as file:
        file.write(message + '\n')

# URL of the website
url = "https://atg.party"

# Check HTTP response code
try:
    response = requests.get(url)
    if response.status_code == 200:
        log("HTTP response code is 200 - OK")
    else:
        log(f"HTTP response code is {response.status_code}")
except Exception as e:
    log(f"Error occurred while checking response code: {e}")

# Create a new instance of Chrome WebDriver
driver = webdriver.Chrome()
driver.implicitly_wait(10)
driver.maximize_window()
# Measure page load time
page_load_time = measure_page_load_time(url)
log(f"Page load time: {page_load_time} seconds")

# Navigate to the website
driver.get(url)
# Click on login element
driver.find_element(By.XPATH, "//span[normalize-space()='Login']").click()

# Enter uid,password and then click signin
driver.find_element(By.ID, "email_landing").send_keys("wiz_saurabh@rediffmail.com")
driver.find_element(By.ID, "password_landing").send_keys("Pass@123")
driver.find_element(By.XPATH, "//*[@id='frm_landing_page_user_login']/div[3]/button").click()

# Navigate to article page
driver.find_element(By.XPATH,"//button[@id='create-btn-dropdown']").click() #click on create
driver.find_element(By.XPATH,"//a[ @class ='atg-p-8 dropdown-item'][normalize-space()='Article']").click() #click on article

# Fill in title and description
title_field = driver.find_element(By.XPATH,"//textarea[@id='title']")
description_field = driver.find_element(By.XPATH,"//div[@class='ce-paragraph cdx-block']")
title_field.send_keys("Test Article Title")
description_field.send_keys("This is a test article description.")

# Upload cover image
element=driver.find_element(By.XPATH,"//*[@id='cover_image']")
element.send_keys("/home/vboxuser/PycharmProjects/myproject/mynew/coverimage.jpg")
# Click on POST
post_button = driver.find_element(By.XPATH,"//button[@id='hpost_btn']")
post_button.click()
# Wait for redirection
time.sleep(3)  # Adjust sleep time as needed

# Log the URL of the new page
new_page_url = driver.current_url
log(f"URL of the new page: {new_page_url}")

driver.quit()

