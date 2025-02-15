---
title: Как редактировать Вики
 
contributors: [positivecrash]
translated: true
---

**Робономика – это набор репозиториев с открытым программным кодом. Корневая команда разработчиков приветствует любые правки со стороны сообщества: исправление багов, опечаток, неясной или устаревшей информации, перевод на любые языки. Вам понадобится аккаунт [GitHub](https://github.com/).**

## Правка уже существующего документа

1. Выберите страницу для редактирования
2. Нажмите на кнопку "Редактировать страницу"
3. Вас переведет на страницу GitHub репозитория, а именно страницу нужного .md файла.
4. При редактировании придерживайтесь общих правил разметки [Markdown](https://ru.wikipedia.org/wiki/Markdown) с учётом некоторых особенностей стека Вики:

### Метаданные
Некоторые сведения о странице в документации указываются в блоке метаданных. Этот блок должен быть расположен вверху markdown файла, использовать корректный синтаксис YAML и ограничен разделителем (---). Вы можете указать следующие параметры:

```YAML
---
title: How to contribute # Заголовок статьи. Заголовок не нужно дублировать в тексте статьи
contributors: [positivecrash] # Основные контрибьюторы (добавляются вручную, исходя из внесенного вклада). Нужно указывать свой Github ник без доп символов
translated: true # "true" если страница переведена на текущий язык (ориентируйтесь на название папки локали, содержащей .md файл)
---
```

### YouTube видео
В документацию можно встраивать YouTube видео без дополнительной разметки, вставив ссылку на видео. Например: `https://youtu.be/kQaSwNYHJQ8`

### Asciinema
Вики Робономики поддерживает встраивание Asciinema. Для этого ознакомьтесь, пожалуйста, со следующей инструкцией:
* Импортируйте компонент сразу после блока метаданных сверху `import Asciinema from '~/components/Asciinema.vue'`
* Вставьте как отдельный параграф `<Asciinema vid="WCFcx8C6M8e52UKDNei1xZloU"/>`, где vid это ID вашего аскикаста.

> You can get the widget script for a specific asciicast by clicking on “Embed” link on asciicast page.
> It looks like this:
> `<script src="https://asciinema.org/a/14.js" id="asciicast-14" async></script>`
[Документация Asciinema](https://asciinema.org/docs/embedding)

В этом примере vid равно 14.

## Добавление нового документа

Если вы хотите добавить новый документ в Вики Робономики:

1. Найдите папку с локалью, соответствующей языку добавляемой статьи, например `/docs/ru/`
2. Создайте файл .md, используя в имени латинские символы и следуйте общим правилам для [структуры url](https://developers.google.com/search/docs/advanced/guidelines/url-structure)
3. Заполните файл, придерживаясь рекомендаций выше
4. Продублируйте созданный файл в папки с другими локалями, даже если вы не планируете переводить документ на другие языки. Не забудьте в неперееденных документах поставить параметр `translated: false`
5. Добавьте документ в меню:
* Откройте файл `/data/sidebar_docs.yaml`
* Выберите место размещения ссылки
* Если вы хотите создать новый раздел, то напишите только заголовок без ссылки
* Добавляйте заголовки только с локалью, для которого есть перевод страницы. Если заголовок указывается для названия раздела, то правило такое: указываем названия только на тех языках, для которых есть хотя бы одна переведенная статья в разделе.
* Для документа добавьте ещё и ссылку. Ссылка должна быть одна для всех языков и не должна содержать код языка. Правильно: `/docs/url-of-your-doc`, неправильно: `/docs/en/url-of-your-doc`
* Для работы с `/data/sidebar_docs.yaml` используйте корректный синтаксис YAML и ориентируйтесь на существующую структуру документа

## Отправьте PR

Отправьте PR с соблюдением некоторых принципов, указанных [здесь](/docs/ru/contributing/#создание-pr).