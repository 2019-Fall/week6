# Week 6
Working with WebDriver and WebElement classes from Selenium and implementing TestCase class from unittest

## Chapter 3. Finding Elements
In this chapter, we will cover the following topics:
  * Understanding more about finding elements with Selenium WebDriver
  * Understanding how to investigate and define locators to find elements using developer tools options available in various browsers
  * Finding out various ways to find elements, including ID, Name, and Class attribute values and use XPath and CSS selectors to define more   dynamic locators
  * Implementing various find_element_by methods to find elements so that

## Installation and Getting Started with Pytest

Pythons: Python 3.5, 3.6, 3.7, PyPy3

Platforms: Linux and Windows

PyPI package name: pytest

Documentation as PDF: [download latest](https://media.readthedocs.org/pdf/pytest/latest/pytest.pdf)

pytest is a framework that makes building simple and scalable tests easy. Tests are expressive and readable—no boilerplate code required. Get started in minutes with a small unit test or complex functional test for your application or library.

### Install pytest
Run the following command in your command line:

    pip install -U pytest

Check that you installed the correct version:

    $ pytest --version
    This is pytest version 5.x.y, imported from $PYTHON_PREFIX/lib/python3.6/site-packages/pytest/__init__.py

### Create your first test
Create a simple test function with just four lines of code:

    # content of test_sample.py
    def func(x):
        return x + 1


    def test_answer():
        assert func(3) == 5

That’s it. You can now execute the test function:

    $ pytest
    =========================== test session starts ============================
    platform linux -- Python 3.x.y, pytest-5.x.y, py-1.x.y, pluggy-0.x.y
    cachedir: $PYTHON_PREFIX/.pytest_cache
    rootdir: $REGENDOC_TMPDIR
    collected 1 item

    test_sample.py F                                                     [100%]

    ================================= FAILURES =================================
    _______________________________ test_answer ________________________________

        def test_answer():
    >       assert func(3) == 5
    E       assert 4 == 5
    E        +  where 4 = func(3)

    test_sample.py:6: AssertionError
    ============================ 1 failed in 0.12s =============================

This test returns a failure report because func(3) does not return 5.

`Note
You can use the assert statement to verify test expectations. pytest’s Advanced assertion introspection will intelligently report intermediate values of the assert expression so you can avoid the many names of JUnit legacy methods.`

### Run multiple tests
pytest will run all files of the form test_*.py or *_test.py in the current directory and its subdirectories. More generally, it follows standard test discovery rules.

### Assert that a certain exception is raised
Use the raises helper to assert that some code raises an exception:

    # content of test_sysexit.py
    import pytest


    def f():
        raise SystemExit(1)


    def test_mytest():
        with pytest.raises(SystemExit):
        f()

Execute the test function with “quiet” reporting mode:

    $ pytest -q test_sysexit.py
    .                                                                    [100%]
    1 passed in 0.12s

### Group multiple tests in a class
Once you develop multiple tests, you may want to group them into a class. pytest makes it easy to create a class containing more than one test:

    # content of test_class.py
    class TestClass:
        def test_one(self):
            x = "this"
            assert "h" in x

        def test_two(self):
            x = "hello"
            assert hasattr(x, "check")

pytest discovers all tests following its Conventions for Python test discovery, so it finds both test_ prefixed functions. There is no need to subclass anything, but make sure to prefix your class with Test otherwise the class will be skipped. We can simply run the module by passing its filename:

    $ pytest -q test_class.py
    .F                                                                   [100%]
    ================================= FAILURES =================================
    ____________________________ TestClass.test_two ____________________________

    self = <test_class.TestClass object at 0xdeadbeef>

        def test_two(self):
            x = "hello"
    >       assert hasattr(x, "check")
    E       AssertionError: assert False
    E        +  where False = hasattr('hello', 'check')

    test_class.py:8: AssertionError
    1 failed, 1 passed in 0.12s

The first test passed and the second failed. You can easily see the intermediate values in the assertion to help you understand the reason for the failure.

read [more](https://docs.pytest.org/en/latest/getting-started.html#request-a-unique-temporary-directory-for-functional-tests)

----
# Week 7

Advanced Web Driver and Web Element class attributes and properties

### Chapter 4. Using the Selenium Python API for Element Interaction 
Web applications use HTML forms to send data to a server. HTML forms contain input elements such as text fields, checkboxes, radio buttons, submit buttons, and more. A form can also contain select lists, text areas, field sets, legends, and label elements.
A typical web application requires you to fill in lots of forms, starting from registering as a user or searching for products. Forms are enclosed in the HTML <'form'> tag. This tag specifies the method of submitting the data, either using the GET or POST method, and the address at which the data entered into the form should be submitted on the server.

In this chapter, we will cover the following topics:
  * Understanding more about the WebDriver and WebElement classes
  * Implementing tests that interact with the application using various methods and properties of the WebDriver and WebElement classes
  * Using the Select class to automate dropdowns and lists
  * Automating JavaScript alerts and browser navigation

#### Elements of HTML forms
![HTML forms](/screenshots/html-dom.png)

#### Examples:
##### Select class for Drop Down Lists

```python
  from selenium.webdriver.support.ui import Select
  
  # get the Your language dropdown as instance of Select class
  select_language = Select(self.driver.find_element_by_id("select-language"))
  
  # check number of options in dropdown
  self.assertEqual(2, len(select_language.options))
  
  # get options in a list for option in select_language.options:
  act_options.append(option.text)
```

##### Alert class for webdriver alerts 

```python
  # click on Remove this item link, this will display
  # an alert to the user
  self.driver.find_element_by_link_text("Clear All").click()

  # switch to the alert
  alert = self.driver.switch_to_alert()
  
  # get the text from alert
  alert_text = alert.text
  
  # check alert text
  self.assertEqual("Are you sure you would like to remove all products from your comparison?", alert_text)
  
  # click on Ok button
  alert.accept()
```

### Chapter 5. Synchronizing Tests
Building robust and reliable tests is one of the critical success factors of automated UI testing. However, you will come across situations where testing conditions differ from one test to another. When your script searches for elements or a certain state of application and it cannot find these elements anymore because the application starts responding slowly due to sudden resource constraints or network latency, the tests report false negative results. We need to match the speed of the test script with the application's speed by introducing delays in the test script. In other words, we need to sync the script with the application's response. WebDriver offers implicit and explicit waits to synchronize tests. 

In this chapter, you will learn about the following topics:
  * Using implicit and explicit wait
  * When to use implicit and explicit wait
  * Using expected conditions
  * Creating a custom wait condition

#### Examples:
```python
  from selenium.webdriver.common.by import By
  from selenium.webdriver.support.ui import WebDriverWait as WDW
  from selenium.webdriver.support import expected_conditions as EC

  first_name = self.driver.find_element_by_id("firstname")
  WebDriverWait(self.driver, 10).until(expected_conditions.visibility_of(first_name))
```
```python
  WebDriverWait(self.driver, 10).until(expected_conditions.visibility_of_element_located((By.ID,"firstname")))
````

### Chapter 9. Advanced Techniques of Selenium WebDriver

So far in the book, we have seen how to set up Selenium WebDriver for testing web applications and some of the important features and APIs for locating and interacting with various elements in the browser. In this chapter, we will explore some of the advanced APIs of Selenium WebDriver. These features come in handy when you're testing fairly complex applications. 

In this chapter, you will learn more about: 
  * Creating tests that simulate keyboard or mouse events using the Actions class
  * Simulating mouse operations such as drag-and-drop and double-click
  * Running JavaScript code
  * Capturing screenshots and movies of test runs
  * Handling browser navigation and cookies

#### Examples:
##### Keyboard actions

```python
  from selenium import webdriver
  from selenium.webdriver.common.by import By
  from selenium.webdriver.support.ui import WebDriverWait
  from selenium.webdriver.support import expected_conditions
  from selenium.webdriver.common.action_chains import ActionChains
  from selenium.webdriver.common.keys import Keys

  # to perform Shift + N key press as if a real user has pressed these keys:
  ActionChains(driver).key_down(Keys.SHIFT).send_keys('n').key_up(Keys.SHIFT).perform()
```
##### Mouse Actions
```python
  # mouse hover over a button and click on the appeared Top link
  mouse_hover_button = self.driver.find_element_by_id('mousehover')
  actions = ActionChains(self.driver)
  actions.move_to_element(mouse_hover_button).perform()
  top_item = self.driver.find_element_by_css_selector(".mouse-hover a[href='#top']")
  actions.move_to_element(top_item).click().perform()

  # mouse double click on box container
  box = driver.find_element_by_tag_name("div")
  ActionChains(driver).move_to_element(driver.find_element_by_tag_name("span")).perform()
  ActionChains(driver).double_click(box).perform()
  
  # mouse drag and drop 
  source = driver.find_element_by_id("draggable")
  target = driver.find_element_by_id("droppable")
  ActionChains(self.driver).drag_and_drop(source, target).perform()
  ```
  ##### Execute Java Script by calling execute_script method of the WebDriver class
  ```python
  # scroll down
  driver.execute_script("Window.ScrollBy(0, 1000);")
  
  # scroll up
  driver.execute_script("Window.ScrollBy(0, -1000);")
  
```

## Steps to clone the project 
1. Copy the url of the repository ending with .git (https://github.com/2019-Fall/week6.git)
2. GitHub Desktop: 
    * Go to Current Repository
    * click on Add drop down
    * Clone Repository
    * click on URL tab
    * paste the copied URL (https://github.com/2019-Fall/week6.git)
    * choose the location from your local machine `C:\dev\` then click on Clone.

    Git Bash: navigate to the right directory `C:\dev\` and enter following:
  ```bash
  git clone https://github.com/2019-Fall/week6.git
  ```

  3. [optional] Create your feature branch: 
  ```bash
  git checkout -b week6_john
  ```
  4. Open the `C:\dev\week6` folder from your VS Code and start modifying the code.

## References
* [Selenium Documentation - Locating Elements](https://selenium-python.readthedocs.io/locating-elements.html#locating-by-id)
* [Python Documentation - Pytest testing framework](https://docs.pytest.org/en/latest/getting-started.html)
* [Learning Selenium Testing Tools with Python](https://www.amazon.com/gp/product/B00RP13D10/ref=dbs_a_def_rwt_hsch_vapi_tkin_p1_i1)
