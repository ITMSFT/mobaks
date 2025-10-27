
[**🇺🇦 Українська версія**](#-українська-версія) | [**🇷🇺 Русская версия**](#-русская-версия)

## 📫 Get in Touch

Our support:

* **🌐 Website:** [itmsft.com](https://itmsft.com/)

* **📧 Email:** [support@itmsft.net](mailto:support@itmsft.net)

* **📞 Phone:** [+38 (048) 706-38-48](tel:+380487063848)

* **🏢 Address:** 1 Hranitna St, Office 42, Odesa, Ukraine

**Our working hours:** Monday — Friday, 12:00 PM – 7:00 PM.

---

[![Превью: Информация по выгрузке](https://img.youtube.com/vi/lHIPin5bhAc/0.jpg)](https://youtu.be/lHIPin5bhAc "Нажмите, чтобы посмотреть видео на YouTube")

## **🇺🇦 Українська версія**

### 1. Загальні положення

**🎯 Мета:** Розробка модуля для системи обліку 1C/BAS для автоматичного вивантаження даних про товари, ціни (включно з гуртовими) та залишки на сайт.

**🔄 Загальна схема інтеграції:**
Інтеграція здійснюється шляхом генерації та вивантаження **одного XML-файлу** на SFTP-сервер. Цей файл містить повну інформацію про каталог, ціни та залишки.

Файл повинен мати **статичне (незмінне) ім'я**.

### 2. 📦 Єдине вивантаження каталогу, цін та залишків (XML)

#### 2.1. Загальні вимоги

* **Формат файлу:** XML (XML-стандарт для каталогів).

* **Кодування:** UTF-8.

* **Ім'я файлу на SFTP:** `catalog.xml`

* **Періодичність:** Налаштовувана, за замовчуванням — кожні 15-30 хвилин (для оперативного оновлення цін та залишків).

#### 2.2. Ключові вимоги до структури

* **Дата генерації:** Атрибут `date` в `<yml_catalog>`.

* **Валюти:** Основна валюта — UAH.

* **Категорії:** Вивантажуються лише українською мовою.

* **Товарні пропозиції (`<offer>`):**

  * **`id`:** Унікальний для кожного SKU.

  * \*\*`group_id`:\*\*Однаковий для варіантів одного товару.

  * **`available`:** Атрибут `true` (якщо `<sklad>` > 0) або `false`.

  * **Мультимовність:** `<name>`, `<description>` (українською) та `<name_ru>`, `<description_ru>` (російською).

  * **Характеристики (`<param>`):** Виключно українською мовою.

  * **Ціна (`<price>`):** Основна роздрібна ціна в UAH.

  * **Залишок (`<sklad>`):** Фактична кількість товару на складі (числове значення).

  * **Гуртові ціни:** Додаткові типи цін (гурт, дилер, VIP тощо) вивантажуються в окремих тегах, згідно зі зразком (напр., `<price1-opt-uah>`, `<price2-dil-usd>` тощо).

#### 2.3. Приклад структури файлу `catalog.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<yml_catalog date="2024-08-06 14:30">
    <shop>
        <name>Назва Вашого Магазину</name>
        <company>ТОВ "Ваша Компанія"</company>
        <url>[https://your-site.com](https://your-site.com)</url>
        <currencies>
            <currency id="UAH" rate="1"/>
            <currency id="USD" rate="39.5"/> <!-- Приклад курсу -->
        </currencies>
        <categories>
            <category id="1">Одяг</category>
            <category id="10" parentId="1">Футболки</category>
        </categories>
        <offers>
            <offer id="tshirt-logo-blue-m" group_id="12345" available="true">
                <name>Фірмова футболка з логотипом</name>
                <name_ru>Фирменная футболка с логотипом</name_ru>
                <url>[https://your-site.com/product/tshirt-logo-blue-m](https://your-site.com/product/tshirt-logo-blue-m)</url>
                
                <!-- Роздрібна ціна -->
                <price>499</price> 
                <currencyId>UAH</currencyId>
                
                <!-- Кількість на складі -->
                <sklad>50</sklad> 
                
                <!-- Додаткові типи цін (приклад зі скріншота) -->
                <price1-opt-uah>450</price1-opt-uah>
                <price2-dil-uah>420</price2-dil-uah>
                <price3-vip-uah>400</price3-vip-uah>
                <price1-opt-usd>23</price1-opt-usd>
                <price2-dil-usd>21</price2-dil-usd>
                <price3-vip-usd>20</price3-vip-usd>
                
                <categoryId>10</categoryId>
                <picture>[https://your-site.com/images/tshirt-blue.jpg](https://your-site.com/images/tshirt-blue.jpg)</picture>
                <description><![CDATA[<p>Якісна футболка.</p>]]></description>
                <param name="Колір">Синій</param>
                <param name="Розмір">M</param>
            </offer>
            <!-- ... (інші товари) ... -->
        </offers>
    </shop>
</yml_catalog>
```

### 3. 🚚 Вимоги до SFTP-з'єднання

* **Хост:** `cloud.itmsft.net`

* **Порт:** `22`

* **Користувач:** `bas`

* **Пароль/Ключ:** `**********` (буде надано окремо)

* **Каталог для вивантаження:** `/path/to/import/`

---
## **🇷🇺 Русская версия**

### 1. Общие положения

**🎯 Цель:** Разработка модуля для системы учета 1C/BAS для автоматической выгрузки данных о товарах, ценах (включая оптовые) и остатках на сайт.

**🔄 Общая схема интеграции:**
Интеграция осуществляется путем генерации и выгрузки **одного XML-файла** на SFTP-сервер. Этот файл содержит полную информацию о каталоге, ценах и остатках.

Файл должен иметь **статичное** (неизменяемое) **имя**.

### 2. 📦 Единая выгрузка каталога, цен и остатков (XML)

#### 2.1. Общие требования

* **Формат файла:** XML (XML-стандарт для каталогов).

* **Кодировка:** UTF-8.

* **Имя файла на SFTP:** `catalog.xml`

* **Периодичность:** Настраиваемая, по умолчанию — каждые 15-30 минут (для оперативного обновления цен и остатков).

#### 2.2. Ключевые требования к структуре

* **Дата генерации:** Атрибут `date` в `<XML_catalog>`.

* **Валюты:** Основная валюта — UAH.

* **Категории:** Выгружаются только на украинском языке.

* **Товарные предложения (`<offer>`):**

  * **`id`:** Уникальный для каждого SKU.

  * **`group_id`:** Одинаковый для вариантов одного товара.

  * **`available`:** Атрибут `true` (если `<sklad>` > 0) или `false`.

  * **Мультиязычность:** `<name>`, `<description>` (на украинском) и `<name_ru>`, `<description_ru>` (на русском).

  * **Характеристики (`<param>`):** Исключительно на украинском языке.

  * **Цена (`<price>`):** Основная розничная цена в UAH.

  * **Остаток (`<sklad>`):** Фактическое количество товара на складе (числовое значение).

  * **Оптовые цены:** Дополнительные типы цен (опт, дилер, VIP и т.д.) выгружаются в отдельных тегах, согласно образцу (напр., `<price1-opt-uah>`, `<price2-dil-usd>` и т.д.).

#### 2.3. Пример структуры файла `catalog.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<yml_catalog date="2024-08-06 14:30">
    <shop>
        <name>Название Вашего Магазина</name>
        <company>ООО "Ваша Компания"</company>
        <url>[https://your-site.com](https://your-site.com)</url>
        <currencies>
            <currency id="UAH" rate="1"/>
            <currency id="USD" rate="39.5"/> <!-- Пример курса -->
        </currencies>
        <categories>
            <category id="1">Одяг</category>
            <category id="10" parentId="1">Футболки</category>
        </categories>
        <offers>
            <offer id="tshirt-logo-blue-m" group_id="12345" available="true">
                <name>Фірмова футболка з логотипом</name>
                <name_ru>Фирменная футболка с логотипом</name_ru>
                <url>[https://your-site.com/product/tshirt-logo-blue-m](https://your-site.com/product/tshirt-logo-blue-m)</url>
                
                <!-- Розничная цена -->
                <price>499</price> 
                <currencyId>UAH</currencyId>
                
                <!-- Количество на складе -->
                <sklad>50</sklad> 
                
                <!-- Дополнительные типы цен (пример со скриншота) -->
                <price1-opt-uah>450</price1-opt-uah>
                <price2-dil-uah>420</price2-dil-uah>
                <price3-vip-uah>400</price3-vip-uah>
                <price1-opt-usd>23</price1-opt-usd>
                <price2-dil-usd>21</price2-dil-usd>
                <price3-vip-usd>20</price3-vip-usd>
                
                <categoryId>10</categoryId>
                <picture>[https://your-site.com/images/tshirt-blue.jpg](https://your-site.com/images/tshirt-blue.jpg)</picture>
                <description><![CDATA[<p>Качественная футболка.</p>]]></description>
                <param name="Колір">Синій</param>
                <param name="Розмір">M</param>
            </offer>
            <!-- ... (другие товары) ... -->
        </offers>
    </shop>
</yml_catalog>
```

### 3. 🚚 Требования к SFTP-соединению

* **Хост:** `cloud.itmsft.net`

* **Порт:** `22`

* **Пользователь:** `bas`

* **Пароль/Ключ:** `**********` (будет предоставлен отдельно)

* **Каталог для выгрузки:** `/path/to/import/`
