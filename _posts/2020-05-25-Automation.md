---
title: GUI Automation with Python
excerpt: >-
  Beginners guide to GUI automation with Python and PyAutoGUI.
date: '2020-05-25'
thumb_img_path: images/snake.png
content_img_path: images/snake.png
layout: post
---


This guide is aimed at beginners who are interested in GUI automation with Python.

See the example project at: [https://github.com/EmElkan/hello-world-automation](https://github.com/EmElkan/hello-world-automation)


## Python

- Install the 3.7.7 version of [Python](https://www.python.org/downloads/)
- Tick Add Python to PATH

![PythonInstall.png](/images/PythonInstall.png)

- Open up Command Prompt as Administrator
- Run command: pip install pyautogui
- Run command: pip install opencv-python

## PyCharm

- Download and install [PyCharm Community Edition](https://www.jetbrains.com/pycharm/download/#section=windows)

![PycharmInstall.png](/images/PycharmInstall.png)

- Open up Pycharm and create a new project named MyProject

![MyProject.png](/images/MyProject.png)

- Go to File > Settings > Project: MyProject > Project Interpreter
- Click the + 

![ProjectInterpreter.png](/images/ProjectInterpreter.png)

- Install packages PyAutoGUI
- Install opencv-python

![PackageInstall.png](/images/PackageInstall.png)

- Close the Available Packages window
- The Project Interpreter should now contain your new packages
- Click OK

![AllPackages.png](/images/AllPackages.png)

- Right click MyProject
- Click New > Python File
- Create a Python File named main_module
- Create a Python File named test_script

![AddFile.png](/images/AddFile.png)

- Right click MyProject
- Click New > Directory
- Create a Directory named Images

![AddFolder.png](/images/AddFolder.png)

## Adding an Image

- Using the Snipping Tool, snip a chunk of the Windows search bar

![ImageSnip.png](/images/ImageSnip.png)

- Save as WindowsSearchBar.png in the MyProject > Images folder

![SavedImage.png](/images/SavedImage.png)


## Creating a Function

![FirstFunction.png](/images/FirstFunction.png)

- Inside main_module import the packages PyAutoGUI and time
```
import pyautogui
import time
````
- Give the function a name (windows_search), and a variable (text) for text input

```
def windows_search(text):
```

- Spend 10 seconds looking for WindowsSearchBar

```
    search = None
    time_end = time.time() + 10
    while search is None and time.time() < time_end:
        try:
            search = pyautogui.locateCenterOnScreen(r'Images/WindowsSearchBar.PNG', grayscale=False, confidence=.8)
            time.sleep(0.5)
        except TypeError:
            print('Looking for Search Bar')

```

- If WindowsSearchBar can not be found print a message and stop the code

```
    if search is None and time.time() > time_end:
        print('Could not find Search Bar')
        exit()

```

- If WindowsSearchBar is found click it and type (text)

```
    print('Found Search Bar')
    pyautogui.click(search)
    pyautogui.typewrite(text)

```

Full code example:

```
import pyautogui
import time


def windows_search(text):
    search = None
    time_end = time.time() + 10
    while search is None and time.time() < time_end:
        try:
            search = pyautogui.locateCenterOnScreen(r'Images/WindowsSearchBar.PNG', grayscale=False, confidence=.8)
            time.sleep(0.5)
        except TypeError:
            print('Looking for Search Bar')
    if search is None and time.time() > time_end:
        print('Could not find Search Bar')
        exit()
    print('Found Search Bar')
    pyautogui.click(search)
    pyautogui.typewrite(text)

```

## Creating a Script

- Inside test_script import main_module

```
from main_module import *

windows_search('paint')
```
- Right click within test_script and click Run 'test_script'

![RunTest.png](/images/RunTest.png)


## Logging

- Right click MyProject
- Click New > Directory
- Create a Directory named Logs
- In main_module import logging
- Add logging messages to windows_search

![Logging.png](/images/Logging.png)

Full code example:
```
def windows_search(text):
    search = None
    time_end = time.time() + 10
    logging.debug('Looking for Windows search bar')
    while search is None and time.time() < time_end:
        try:
            search = pyautogui.locateCenterOnScreen(r'Images/WindowsSearchBar.PNG', grayscale=False, confidence=.8)
            time.sleep(0.5)
        except TypeError:
            print('Looking for Search Bar')
    if search is None and time.time() > time_end:
        print('Could not find Search Bar')
        logging.error('Could not find Search Bar')
        exit()
    print('Found Search Bar')
    logging.debug('Found Windows Search Bar')
    pyautogui.click(search)
    pyautogui.typewrite(text)


```

- In test_script add logging configuration

```
logging.basicConfig(filename='Logs/test_script.log',
                    format='%(asctime)s %(levelname)s [%(module)s] [%(funcName)s]: %(message)s',
                    datefmt='[%Y.%m.%d] %H:%M:%S -', level=logging.DEBUG)
```

- Add logging messages

![LoggingConfig.png](/images/LoggingConfig.png)



```
from main_module import *

logging.basicConfig(filename='Logs/test_script.log',
                    format='%(asctime)s %(levelname)s [%(module)s] [%(funcName)s]: %(message)s',
                    datefmt='[%Y.%m.%d] %H:%M:%S -', level=logging.DEBUG)

logging.info('script has begun')

windows_search('paint')

print('Finished')
logging.info('Script has finished')

```
- Right click within test_script and click Run 'test_script'
- Open up Logs to see test_script.log

## Screenshots

- Right click MyProject
- Click New > Directory
- Create a Directory named Screenshots

To take a screenshot when the Windows search bar can't be found, add the below command in main_module under the if statement in windows_search:

```
pyautogui.screenshot(r'Screenshots/CouldNotFindWindowsSearchBar.png')
```

![Screenshots.png](/images/Screenshots.png)

Now when we run test_script if the program can not find Windows search bar it will take a screenshot:

![CouldNotFind.png](/images/CouldNotFind.png)

## Useful Links

- [PyAutoGui Docs](https://pyautogui.readthedocs.io/en/latest/)
- [Python for Beginners](https://www.python.org/about/gettingstarted/)
- [Pycharm Quick Start Guide](https://www.jetbrains.com/help/pycharm/quick-start-guide.html)


&nbsp;

*Illustration by [Icons 8](https://icons8.com/)*
