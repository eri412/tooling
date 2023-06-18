# **Анализ https://www.gd.ru/articles/9039-finansovyy-kontrol**

## **1. Network**

### **1.1 Профиль загрузки ресурсов**

[HAR](./profiles/www.gd.ru.har)

### **1.2 Неоптимальные места**

#### **1.2.1 Дублирование ресурсов**

![bootstrap](./images/bootstrap.png)

![cast_and_code](./images/cast_and_code.png)

Не уверен, являются ли нижние jquery дубликатами, или это какие-то плагины.

![jquery](./images/jquery.png)

![openapi](./images/openapi.png)

![popper](./images/popper.png)

![bootstrap_again](./images/bootstrap_again.png)

![fonts](./images/fonts.png)

#### **1.2.2 Лишний размер ресурса**

Очень много элементов, состоящих из текста, загружаются и рендерятся в виде **растровых картинок(!)**.

Например:

1. Лого сайта, полностью состоящее из текста, это PNG

![logo_img](./images/logo_img.png)

![logo_load](./images/logo_load.png)

2. В шапке сайта есть целый список PNG ссылок с обычным текстом

![head_img](./images/head_img.png)

![head_load](./images/head_load.png)

В html остались комменты

![html_comments](./images/html_comments.png)

В jquery остались комменты

![jquery_comments](./images/jquery_comments.png)

Многие js файлы не минифицированны.

#### **1.2.3 Медленно загружающиеся ресурсы**

Если отсортировать ресурсы по времени загрузки, то можно заметить, что очень многие из них большинство времени провели в DNS lookup.

![dns](./images/dns.png)

![dns_load](./images/dns_load.png)

#### **1.2.4 Ресурсы, блокирующие загрузку**

async скрипты в head заблочат парсинг во время исполнения (но не во время загрузки), поэтому могут возникнуть задержки.
![async](./images/async.png)

#### **1.2.5 Другое**

Много зафейленных запросов

![fail](./images/fail.png)

Много редиректов

![redirect](./images/redirect.png)

Картинка в начале статьи грузится лениво, хотя любой типичный посетитель до неё долистает. Из-за этого видно, как она рендерится во время чтения сайта.

![lazy_img](./images/lazy_img.png)

![lazy_dom](./images/lazy_dom.png)

----------

## **2. Perfomance**
### **2.1 Профиль**

[JSON](./profiles/Coverage-20230618T201917.json)

### **2.2 Время до FP, FCP, LCP, DCL, Load**

FP - 585.3 ms

![fp](./images/fp.png)

FCP - 585.3 ms

![fcp](./images/fcp.png)

DCL - 1378.7 ms

![dcl](./images/dcl.png)

LCP - 1602 ms

![lcp](./images/lcp.png)

Load - 41739 ms

![load](./images/load.png)

### **2.3 Элемент, на котором происходит LCP**

Упомянутая выше лениво загружаемая картинка и есть LCP. Причем иногда она загрузится до DCL, иногда после, в зависимости от того, закрыт ли был по дефолту верхний баннер, и появилось ли модальное окно или нет.

### **2.4 Время на разные этапы обработки документа**

- Loading - 41 ms

- Scripting - 1531 ms

- Rendering - 566 ms

- Painting - 38 ms

![stats](./images/stats.png)

----------

## **3. Coverage**
### **3.1 Профиль**

[JSON](./profiles/Coverage-20230618T201917.json)

![Скриншот](./images/coverage.png)
### **3.2 Неиспользованный CSS**

~435 kB

### **3.3 Неиспользованный JS**

~2.27 MB

----------

Всего из 4.3 MB не было использовано 2.7 MB. Используются всего 36% из загруженных ресурсов.

----------
