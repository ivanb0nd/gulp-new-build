# Gulp - автор MaxGraph - https://github.com/maxdenaro

> Используется Gulp 4

## Начало работы

Для работы с данной сборкой в новом проекте, склонируйте все содержимое репозитория <br>
`git clone <this repo>`
Затем, находясь в корне проекта, запустите команду `npm i`, которая установит все находящиеся в package.json зависимости.
После этого вы можете использовать любую из четырех предложенных команд сборки (итоговые файлы попадают в папку __app__ корневой директории): <br>
`gulp` - базовая команда, которая запускает сборку для разработки, используя browser-sync


`gulp build` - команда для продакшн-сборки проекта. Все ассеты сжаты и оптимизированы для выкладки на хостинг.

`gulp cache` - команда, которую стоит запускать после `gulp build`, если вам нужно загрузить новые файлы на хостинг без кэширования.

`gulp backend` - специальная команда для создания сборки под дальнейшее бэкенд-взаимодействие. Подробнее об этом ниже.

## Структура папок и файлов

```
├── src/                          # Исходники
│   ├── js                        # Скрипты
│   │   └── main.js               # Главный скрипт
│   │   ├── components            # js-компоненты
│   │   ├── functions             # готовые, частоиспользуемые функции для сайта
│   │   ├── vendor                # папка для загрузки локальных версий библиотек
│   ├── scss                      # Стили сайта (препроцессор sass в scss-синтаксисе)
│   │   └── main.scss             # Главный файл стилей
│   │   └── global.scss           # Файл с глобальными настройками - сбросы, шрифты и т.д.
│   │   └── vendor.scss           # Файл для подключения стилей библиотек из папки vendor
│   │   └── _fonts.scss           # Файл для подключения шрифтов (можно использовать миксин)
│   │   └── _mixins.scss          # Файл для подключения миксинов из папки mixins
│   │   └── _vars.scss            # Файл для написания css- или scss-переменных
│   │   └── _settings.scss        # Файл для написания глобальных стилей
│   │   ├── components            # scss-компоненты
│   │   ├── mixins                # папка для сохранения готовых scss-компонентов
│   │   ├── vendor                # папка для хранения локальных css-стилей библиотек
│   ├── partials                  # папка для хранения html-частей страницы
│   ├── img                       # папка для хранения картинок
│   │   ├── svg                   # специальная папка для преобразования svg в спрайт
│   ├── resources                 # папка для хранения иных ассетов - php, видео-файлы, favicon и т.д.
│   │   ├── fonts                 # папка для хранения шрифтов в формате woff2
│   └── index.html                # Главный html-файл
└── gulpfile.js                   # файл с настройками Gulp
└── package.json                  # файл с настройками сборки и установленными пакетами
└── .editorconfig                 # файл с настройками форматирования кода
└── .stylelintrc                  # файл с настройками stylelint
└── README.md                     # документация сборки
```

## Оглавление
1. [npm-скрипты](#npm-скрипты)
2. [Работа с html](#работа-с-html)
3. [Работа с CSS](#работа-с-css)
4. [CSS-миксины](#css-миксины)
5. [Работа с JavaScript](#работа-с-javascript)
6. [Работа со шрифтами](#работа-со-шрифтами)
7. [Работа с изображениями](#работа-с-изображениями)
8. [Работа с иными ресурсами](#работа-с-иными-ресурсами)
9. [Backend-скрипт](#backend-скрипт)


## npm-скрипты

Вы можете вызывать gulp-скрипты через npm.
Также в сборке есть возможность проверять код на соответствие конфигу (editorconfig) и валидировать html.

`npm run html` - запускает валидатор html, запускать нужно при наличии html-файлов в папке __app__.

`npm run code` - запускает editorconfig-checker для проверки соответствия конфиг-файлу.

## Работа с html

Благодаря плагину __gulp-file-include__ вы можете разделять html-файл на различные шаблоны, которые должны храниться в папке __partials__. Удобно делить html-страницу на секции.

> Для вставки html-частей в главный файл используйте `@include('partials/filename.html')`

Если вы хотите создать многостраничный сайт - копируйте __index.html__, переименовывайте как вам нужно, и используйте.

При использовании команды `gulp build`, вы получите минифицированный html-код в одну строку для всех html-файлов.

## Работа с CSS

В сборке используется препроцессор __sass__ в синтаксисе __scss__.

Стили, написанные в __components__, следует подключать в __main.scss__.
Стили из ___fonts__, ___settings__, ___vars__ и ___mixins__ подключены в __global.scss__.

Чтобы подключить сторонние css-файлы (библиотеки) - положите их в папку __vendor__ и подключите в файле __vendor.scss__

Если вы хотите создать свой миксин - делайте это в папке __mixins__, а затем подключайте в файл ___mixins.scss__.

Если вы хотите использовать scss-переменные - подключите ___vars.scss__ также в main.scss или в любое другое место, где он нужен, но обязательно удалите __:root__.

> Для подключения css-файлов используйте директиву `@import`

В итоговой папке __app/css__ создаются три файла: <br> __main.css__ - для стилей страницы, <br> __global.css__ - для глобальных стилей, <br> __vendor.css__ - для стилей всех библиотек, использующихся в проекте.

При использовании команды `gulp build`, вы получите минифицированный css-код в одну строку для всех css-файлов. 

## CSS-миксины

В сборку будут добавляться готовые scss-миксины для различных компонентов, например для чекбокса или бургера. Миксины уже готовы, а вот html (и js, если нужен) код к ним написан в соответствующих файлах в папке __parts__. Данная папка никак не обрабатывается gulp, потому что в ней просто лежат html-примеры, которые вы можете при желании сделать сниппетами.

Готовый html можно вставлять в любой html-файл, а js-код копировать в соответствующие файлы в __js/components__

## Работа с JavaScript

Поддержка `import` и `require` не реализована! Файлы собираются автоматически из различных папок.

JS-код лучше делить на компоненты - небольшие js-файлы, которые содержат свою, изолированную друг от друга реализацию. Такие файлы помещайте в папку __components__.

В папке __functions__ лежат (и будут появляться новые) файлы с готовым кодом различных часто встречающихся кейсов, будь то использование бургер-меню, реализация табов и т.д. Вам останется лишь поменять селекторы в коде на свои.

В файле __main.js__ ничего не подключается, он рекомендуется для реализации общей логики сайта.

Чтобы подключить сторонние js-файлы (библиотеки) - положите их в папку __vendor__.

При использовании команды `gulp build`, вы получите минифицированный js-код в одну строку для всех js-файлов.

## Работа со шрифтами

Т.к. автор не поддерживает IE11, в сборке реализована поддержка только формата __woff2__ (это значит, что в миксине подключения шрифтов используется только данный формат).

Загружайте файлы __woff2__ в папку __resources/fonts__, а затем вызывайте миксин `@font-face` в файле ___fonts.scss__.

## Работа с изображениями

Любые изображения, кроме __favicon__ кладите в папку __img__.

Если вам нужно сделать svg-спрайт, кладите нужные для спрайта svg-файлы в папку __img/svg__. Иные svg-файлы просто оставляйте в папке __img__.

При использовании команды `gulp build`, вы получите минифицированные изображения в итоговой папке __img__.

## Работа с иными ресурсами

Любые ресурсы (ассеты) проекта, под которые не отведена соответствующая папка, должны храниться в папке __resources__. Это могут быть видео-файлы, php-файлы (как, например, файл отправки формы), favicon и прочие.

## Backend-скрипт

На данный момент реализована отдельная версия сборки под backend, которая похожа на `gulp build`, однако отличается отсутствием минификации итоговых файлов, что при передаче backend-специалисту просто не нужно.

Важно понимать, что если вы разрабатываете сайт под дальнейшее backend-взаимодействие, стоит сразу разбивать css-файлы по-другому. Вместо использования папки __components__, создавайте css-файлы напрямую рядом с __main.scss__, подключайте к html-файлу.

Также важно, что при таком взаимодействии нужно подключать js-файлы из папки __components__ также напрямую к html-файлу.


