# Robot Framework

### Introduction

Robot Framework is a generic open source automation framework. It is a keyword-driven testing framework which use easy syntax, utilizing human-readable keywords. The first version was developed at Nokia Networks in 2005.  It is released under Apache License 2.0 and can be downloaded from robotframework.org.

The framework is written using the Python programming language and has an active community of contributors. Robot Framework libraries are best implemented in Python, but using Java or .NET is also possible.

### Resources

- Robot Framework website: https://robotframework.org/
- User Guide: https://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html
- For Selenium Library: https://robotframework.org/SeleniumLibrary/SeleniumLibrary.html
- CLI Commands: https://dev.to/juperala/how-to-run-robot-framework-test-from-command-line-5aa

### Installation

1. Install Python

   -  https://www.python.org/downloads/

2. Confirm installation

   ```bash
   python --version
   ```

   ```bash
   pip --version
   ```

3. Install Robot Framework

   ```bash
   pip install robotframework
   ```

4. Install Selenium Library for Robot Framework

   ```bash
   pip install --upgrade robotframework-seleniumlibrary
   ```

5. Download Selenium web drivers

   - For Chrome: https://chromedriver.chromium.org/
   - For Firefox: https://github.com/mozilla/geckodriver/releases

6. Add web drivers to PATH

### Recommended Tools

- **PyCharm**: https://www.jetbrains.com/pycharm/
- **RIDE**: https://github.com/robotframework/RIDE/wiki

### Sample Code written in Python

A script for the simple test written in Python, which opens a browser, goes to daraz.pk and search for football in a search field.

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

driver = webdriver.Firefox()
driver.get("https://www.daraz.pk")
time.sleep(3)

search = driver.find_element_by_id("q")
search.send_keys("Football")
search.send_keys(Keys.RETURN)
time.sleep(3)

driver.close()
```

### Sample Code written with Robot framework

A script for the simple test written with Robot Framework, which opens a browser, goes to daraz.pk and search for football in a search field.

```python
*** Settings ***
Documentation  Validate the search result of daraz.pk 
Library  SeleniumLibrary

*** Test Cases ***
User can search for products
    [Tags]  Smoke
    Open Browser  https://www.daraz.pk  firefox
    Wait Until Page Contains  Sell on Daraz
    Input Text  id=q  Football
    Click Button  SEARCH
    Wait Until Page Contains  Search Results
    Close Browser
```

### Folder Structure

1. Tests ( For Test Scripts)
2. Resources (For Keywords file and Page Objects)
3. Results (For Reports)

### File Format

- In Tests folder, save file as: **first_test.robot**

### Sections of the Script:

1. Settings
2. Variables
3. Test Cases
4. Keywords (optional)

### CLI Commands

- To save reports in specific folder:

```bash
robot -d results tests/amazon.robot
```

- To run specific tags in specific test:

```bash
robot -i Smoke tests/amazon.robot
```

### Example

##### Scenario: 

To test the Sign In functionality of the website by entering valid email and valid password.

##### Test Case: 

**TCID101 | Test Case 1| Happy Path Sign In**
**Title:** Enter correct email address and password.
**Inputs:** url: https://react-redux.realworld.io/ | email: john91@gmail.com | password: qwerty123
**Action:** Enter the given URL in browser. At home page click on "Sign In" button at top. When "Sign In" page is open enter valid email and valid password. Click on "Sign In" Button. 
**Expected Output:** Go to the dashboard page.
**Latest Result:** Pass
**Need to be Automated:** Yes

##### Test Script:

```python
*** Settings ***
Documentation  Verify the SignIn functionality
Library  SeleniumLibrary

*** Test Cases ***
User should SignIn to the dashboard with valid email and valid password
    [Documentation]  Happy Path of Login
    [Tags]  Smoke
    Open Browser    https://react-redux.realworld.io/  firefox
    Wait Until Page Contains    conduit
    Click Element   xpath=//*[@id="main"]/div/nav/div/ul/li[2]/a
    Wait Until Page Contains    Sign In
    Input Text  xpath=/html/body/div/div/div/div/div/div/form/fieldset/fieldset[1]/input     john91@gmail.com
    Input Text  xpath=/html/body/div/div/div/div/div/div/form/fieldset/fieldset[2]/input     qwerty123
    Click Button    Sign in
    Wait Until Page Contains    No articles are here... yet.
    CLose Browser
```



------



## Robot Framework with Gherkin

### Introduction

Robot Framework supports BDD style test cases, this support is limited to Given/When/Then keywords. Features like data tables and docstrings are not supported in Robot Framework. Steps definitions are written in the Keyword section.

### Resources

- Gherkin Reference: https://cucumber.io/docs/gherkin/reference/

### Example with Gherkin

##### Test Case:

```gherkin
Feature: Verify the user signin feature
    The user should be able to signin into the application when the email and the password are correct.

    Scenario: Entering valid sigin credentials
        Given I am on Home page
        When I click on Signin
        Then Signin page opens
        When I enter valid credentials:
        | email | password |
    	| john91@gmail.com | qwerty123 |
        And I click on Signin button
        Then Dashboard page opens
        
    	
```

##### Test Script (Gherkin)

```python
*** Settings ***
Documentation  To Verify the SignIn functionality
Library  SeleniumLibrary
Suite Setup  Open New Browser
Suite Teardown  Close Browser

*** Variables ***
${URL}  https://react-redux.realworld.io/
${BROWSER}  chrome

*** Test Cases ***
User should SignIn to the dashboard with valid email and valid password
    [Documentation]  Happy Path of Login
    [Tags]  Smoke
    Given I am on Home page
    When I click on Signin
    Then Signin page opens
    When I enter valid credentials
    And I click on Signin button
    Then Dashboard page opens

*** Keywords ***
I am on Home page
    Go To    ${URL}
    Wait Until Page Contains    conduit

I click on Signin
    Click Element   xpath=//*[@id="main"]/div/nav/div/ul/li[2]/a

Signin page opens
    Wait Until Page Contains    Sign In

I enter valid credentials
    Input Text  xpath=//*[@id="main"]/div/div/div/div/div/form/fieldset/fieldset[1]/input    john91@gmail.com
    Input Password  xpath=//*[@id="main"]/div/div/div/div/div/form/fieldset/fieldset[2]/input     qwerty123

I click on Signin button
    Click Button    Sign in

Dashboard page opens
    Wait Until Page Contains    No articles are here... yet.

Open New Browser
    Open Browser  about:blank  ${BROWSER}
```

### Recommended Tool

- **gherkin2robotframework**: https://pypi.org/project/gherkin2robotframework/
