# BIN Checker

Этот проект — автоматизированный тест на Python + Selenium, который проверяет компании по БИН из файла и собирает их статусы с вкладки **"Надёжность"**.

---

## 🚀 Возможности
- Авторизация на сайте  
- Поиск компании по БИН  
- Переход в профиль  
- Сбор всех статусов с вкладки "Надёжность"  
- Сохранение результатов в txt


## 🔧 Как сделать проверку универсальной (для всех вкладок)

Скрипт сейчас собирает только данные с вкладки **"Надёжность"**.  
Чтобы сделать его универсальным и собирать данные со всех вкладок:

1. В файле `pages/reliability_page.py`:
   - Переименуйте класс в более общий, например:
     ```python
     class CompanyProfilePage:
     ```
   - Для каждой вкладки сделайте отдельный метод:
     ```python
     def open_tab(self, tab_name):
         tab = self.driver.find_element("xpath", f"//a[contains(text(), '{tab_name}')]")
         tab.click()
         time.sleep(2)

     def get_tab_data(self):
         elements = self.driver.find_elements("xpath", "//div[@class='status-block']")
         return [el.text for el in elements]
     ```

2. В `test_check_customs_status.py` замените:
   ```python
   reliability_page.open_tab()
   status = reliability_page.get_customs_status()

3. В `test_check_customs_status.py` добавьте цикл для всех вкладок:
   ```python
   tabs = ["Надёжность", "Финансы", "Отзывы"]  # Добавьте все нужные вкладки
   for tab in tabs:
       profile_page.open_tab(tab)
       data = profile_page.get_tab_data()
       print(f"Data from {tab}: {data}")
   ```
4. В `test_check_customs_status.py` обновите импорт:
   ```python

5. В get_bin можно получить БИНЫ из pdf файла за