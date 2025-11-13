# Практические задания

## Задание 1: Простой тест авторизации

Выполнить задание ниже используя теорию "Основные методы Selenium WebDriver в Python"
Весь код пока пишем одним файлом, без классов, функций, фикстур, просто дописать шаблон
Для написания локаторов в целях обучения попробовать разные методы, By.ID, By.XPATH и т.п.

1. Установить браузер chrome (обычный, вебдрайвер селениум сам подтянет)
2. Установить pycharm, создать проект, создать файл `test_login.py`, вставить в файл шаблон решения представленный ниже
3. Установить selenium через наведение мыши в IDE на строках импорта (from selenium import webdriver)
4. Комментарии "# 1." оставляем, требуемый код дописываем ниже каждого комментария
5. Для проверки текста использовать assert. Есть ньюанс с проверкой текста, добиться именно успешного прохождения, считаем что не баг, а особенность.

Практическое задание

Задача: Автоматизировать сценарий:

1. Открыть https://the-internet.herokuapp.com/login
2. Ввести логин tomsmith и пароль SuperSecretPassword!
3. Нажать Login
4. Дождаться сообщения "You logged into a secure area!"
5. Нажать Logout

Шаблон решения:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
try:
    driver.get("https://the-internet.herokuapp.com/login")
    
    # Ваш код здесь
    # 1. Найти поля ввода и кнопку
    # 2. Ввести учетные данные
    # 3. Кликнуть Login
    # 4. Ожидать появления flash-сообщения (id="flash")
    # 5. Проверить текст сообщения
    # 6. Найти и нажать Logout
    
finally:
    driver.quit()
```

## Задание 2: Добавление в проект pytest

Добавим в начало файла

```python
import pytest
```

Вынесем написанный нами код (все кроме импортов) в функцию и добавим класс

Важно!

- Имена тестовых классов - Должны начинаться с Test (с большой буквы)
- Имена тестовых функций - Должны начинаться с test_

Добавьте любую pytest метку для теста

в результате должно получиться так:

```python
class TestLoginPage:

    @pytest.mark.smoke
    def test_login(self):
        driver = webdriver.Chrome()
        try:
# остальной код без изменений не считая отступов
```

Написать по аналогии второй тест, пока отдельной функцией `def test_login_invalid(self):`
Тест на ввод неправильного логина с проверкой сообщения "Your username is invalid!"

Проверьте что у обоих тестов указана одна и та-же метка `@pytest.mark.smoke`

Создайте файл `pytest.ini` в корне проекта и скопируйте в него следующее:

```
[pytest]

# Указывается шаблон имен файлов для обнаружения тестов
python_files =
    test_*.py
 
# Указывается шаблон имен для обнаружения тест-классов
python_classes = Test*

# Указывается шаблон имен для обнаружения тест-методов
python_functions = test_*

# Добавление меток (например, для запуска тестов с определёнными метками)
markers =
    regress: метки для регресс тестов
    smoke: метки для smoke-тестов

# Указывает на директорию для хранения временных файлов
cache_dir = .pytest_cache

# Игнорировать эти директории при поиске тестов
norecursedirs = .git venv
```

Запустите тесты через терминал в pycharm командой:
```
pytest -m smoke
```

## Задание 3: Вынос переменных и локаторов элементов из тестов

Теперь когда у нас два теста использующих одни и те-же переменные и локаторы элементов вынесем их из тестов. Вынесем только то, что обозначено ниже:

```python
class TestLoginPage:
    # Конфигурационные параметры
    BASE_URL = "https://the-internet.herokuapp.com/login"
    WAIT_TIMEOUT = 15

    # Локаторы элементов
    class Locators:
        USERNAME_INPUT = (By.ID, "username")
        PASSWORD_INPUT =
        LOGIN_BUTTON =
        FLASH_MESSAGE = 
        LOGOUT_BUTTON = 

    # Текстовые константы
    class Messages:
        SUCCESS_LOGIN = "You logged into a secure area!"
        INVALID_USERNAME = "Your username is invalid!"
```
В коде самих тестов заменить прямое использование на переменные. 


В случаях с обычными переменными все просто, мы заменяем значение в тесте на переменную через self

```python
driver.get(self.BASE_URL)
```
Если переменная добавлена в класс:

```python
self.класс.переменная
```

С локаторами немного сложнее:

```python
username = driver.find_element(*self.Locators.USERNAME_INPUT)
```
Звездочка * в Python используется для распаковки кортежа (tuple unpacking).

В нашем случае:

```python
username = driver.find_element(*self.Locators.USERNAME_INPUT)
```
Эквивалентно:

```python
username = driver.find_element(By.ID, "username")
```
Почему это нужно:

- Локатор хранится как кортеж: USERNAME_INPUT = (By.ID, "username")
- Метод find_element ожидает два отдельных аргумента: find_element(by, value)
- Распаковка преобразует: *(By.ID, "username") → By.ID, "username"

### Опции браузера

При запуске тестов можно столкнуться с сообщениями со стороны браузера которые будут препятствовать успешному прохождению тестов. Как вариант мы можем блокировать уведомления браузера. Создадим для этого функцию для настройки браузера:

```python
    def configure_chrome_driver(self):
        chrome_options = Options()

        # Настройки для отключения сохранения паролей и автозаполнения
        prefs = {
            "credentials_enable_service": False,
            "profile.password_manager_enabled": False,
            "profile.default_content_setting_values.notifications": 1  # Пример: блокировка уведомлений
        }
        chrome_options.add_experimental_option("prefs", prefs)

        driver = webdriver.Chrome(options=chrome_options)
        return driver
```
Есть масса других параметров и опций которые нам могут потребоваться, подробно пока останавливаться на них не будем, рассмотрим это немного позже.


## Задание 4: Добавление в проект параметризации

Хоть мы и вынсли переменные из тестов - дублирования кода остается много. 
Видно что мы получили два теста с одинаковой логикой, большая часть кода в которых совпадает, частично уйти от этого нам поможет параметризация.

Следующий шаг обьединить эти два теста в один.

Доработаем тест test_login

1. Добавить в функцию на вход три параметра 

```python
def "test_login(self, login, password, expected_result):"
```

через `expected_result` мы будем определять что мы ожидаем - сообщение об успехе или ошибку

2. Изменить в "2. Ввести учетные данные" ввод не конкретных значений а переменные login, password
3. Добавить перед обьявлением функции параметризацию

```python
    @pytest.mark.parametrize("login, passwd, expected_result",
        [
            ("tomsmith", "SuperSecretPassword!", True), # Валидные данные
            (), # Неправильный пароль
        ]
    )
```

1. Сначала мы обозначаем какие переменные мы будем передавать `"login, passwd, expected_result"`
2. Затем перечисляем конкретные значения этих переменных `"tomsmith", "SuperSecretPassword!", True`

Рассмотрим только эти два кейса:
1. Валидные данные
2. Неправильный логин

ps: Тут есть еще вариант с неправильным паролем и правильным логином, но на нормальных ресурсах не делают отдельное сообщение для таких ситуаций и для этого нужно немного менять подход к параметризации. 

Что бы выполнять один из двух assert'ов можно использовать "if expected_result:" более наглядно это можно представить как "if expected_result == True:" просто более правильное и лаконичное написание

Не забудте учесть что кнопки Logout без успешной авторизации не будет

```python
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.options import Options


class TestLoginPage:

    def configure_chrome_driver(self):
        chrome_options = Options()

        # Настройки для отключения сохранения паролей и автозаполнения
        prefs = {
            "credentials_enable_service": False,
            "profile.password_manager_enabled": False,
            "profile.default_content_setting_values.notifications": 1  # Пример: блокировка уведомлений
        }
        chrome_options.add_experimental_option("prefs", prefs)

        driver = webdriver.Chrome(options=chrome_options)
        return driver

    @pytest.mark.smoke
    @pytest.mark.parametrize("login, passwd, expected_result",
        [
            ("tomsmith", "SuperSecretPassword!", True), # Валидные данные
            ("login", "password", False), # Неправильный пароль
        ])
    def test_login(self, login, passwd, expected_result):

        driver = self.configure_chrome_driver()
        try:
            driver.get("https://the-internet.herokuapp.com/login")

            # 1. Найти поля ввода и кнопку
            username = driver.find_element(By.ID, "username")
            password = driver.find_element(By.ID, "password")
            login_button = driver.find_element(By.TAG_NAME, "button")

            # 2. Ввести учетные данные
            username.send_keys(login)
            password.send_keys(passwd)

            # 3. Кликнуть Login
            login_button.click()

            # 4. Ожидать появления flash-сообщения (id="flash")
            wait = WebDriverWait(driver, 15)
            message = wait.until(EC.visibility_of_element_located((By.ID, "flash")))

            # 5. Проверить текст сообщения
            message_clear = ' '.join(message.text.strip().replace('×', '').split())
            if expected_result:
                assert message_clear == "You logged into a secure area!", f'Expected "You logged into a secure area!", got "{message_clear}"'

                # 6. Найти и нажать Logout
                logout_button = driver.find_element(By.LINK_TEXT, "Logout")
                logout_button.click()
            else:
                assert message_clear == "Your username is invalid!", f'Expected "Your username is invalid!", got "{message_clear}"'


        finally:
            driver.quit()
```

## Задание 5: Добавление в проект фикстуры

Мы можем вынести инициализацию и закрытие браузера в фикстуру, используя pytest.
Создадим фикстуру, которая будет запускать браузер перед каждым тестом и закрывать после.

Создадим фикстуру driver до обьявления класса

```python
@pytest.fixture
def driver():
    # открыть браузер
    # перейти на страницу
    yield driver
    # закрыть браузер после завершения теста
```

В итоге должно получиться так:

```python
@pytest.fixture
def driver():
    driver = webdriver.Chrome()
    driver.get("https://the-internet.herokuapp.com/login")
    yield driver
    driver.quit()
```

и теперь нашим тестам нужно сообщить о существовании `driver`

```python
def test_login(self, driver):
```

Убедитесь что убрали из тестов все что связано с браузером, т.е. то что описано в фикстуре `driver`
Весь код фикстуры до `yield` выполнится перед выполнением кода каждого теста
Весь код фикстуры после `yield` выполнится после выполнения кода каждого теста

## Задание 6: Изменение структуры проекта по Page Object

Cоздайте файл login_page.py

В нем создадим класс LoginPage

Вынесем конфигурационные параметры и локаторы элементов из test_login.py в login_page.py

Фикстуру и BASE_URL пока оставляем в test_login.py

В login_page.py создадим функцию login
В нее нужно переместить все что находится в функции test_login

переместить часть импортов в login_page.py

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
```

В файл test_login.py импортируем класс LoginPage из login_page

```python
from login_page import LoginPage
```
И вызовем функцию login в тесте

```python
page = LoginPage()
page.login(driver, login, passwd, expected_result)
```

Cоздайте файл conftest.py

И вынесем в него фикстуру `driver`

Cоздайте файл config.py

Вынесем в него переменные BASE_URL и WAIT_TIMEOUT

Добавьте импорты

```
from config import BASE_URL
```

```
from config import WAIT_TIMEOUT
```
Там где используются эти переменные, self при из вызове не нужен

Убедитесь что тесты запускаются через терминал

## Задание 7: Написание оберток над функциями selenium и добавление отчетности Allure

Allure
Screenshot

