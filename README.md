# Второй выпуск React Challenge

## Установка стартового шаблона
1. Форкните данный репозиторий
2. Клонируйте свой форк с помощью команды `git clone https://github.com/<account>/react-challenge-colorizr`, вместо `<account>` подставьте имя своего профиля на Github, например, `https://github.com/rtivital/react-challenge-colorizr`
3. Откройте папку, в которую был клонирован репозиторий `cd react-challenge-colorizr`
4. Установите зависимости с помощью команды `npm install`
5. Выполните команду `npm start` для проверки работоспособности приложения. Если всё прошло хорошо, то вы найдёте результат по адресу [`localhost:3000`](http://localhost:3000/). После запуска сервера [Webpack](https://webpack.github.io/) будет отслеживать все изменения, происходящие с js файлами в папке `src/` и автоматически перезагружать страницу при их изменении.
6. Вы восхитительны и готовы написать свой сервис для работы с цветом на React

## Линтинг
Для проверки кода в проекте используется [ESLint](http://eslint.org/). Из коробки ESLint не поддерживает проверку кода, написанного с использованием React, поэтому необходимо использовать специальный [плагин](https://github.com/yannickcr/eslint-plugin-react). Все настройки, по которым будет производиться проверка кода можно найти (и при желании доработать) в файле `.eslintrc`.

Для запуска линтинга воспользуйтесь командой

```sh
npm run lint
```

## Сборка проекта
Проект собирается с помощью [Webpack](https://webpack.github.io/). JavaScript код обрабатывается [Babel](https://babeljs.io/). Также по умолчанию к проекту подключен лоадер для компилирования и сборки [Sass](http://sass-lang.com/) (как наиболее популярный препроцессор).

Настройки для Webpack хранятся в двух файлах:
* `webpack.config.js` — конфиг, который используется только во время разработки
* `webpack.config.prod.js` — конфиг, используемый при выводе приложения в "продакшен"

Запуск Webpack с разными конфигами осуществляется разными способами. При разработке приложения используется автоматическая перезагрузка, при этом создаётся виртуальный файл `public/bundle.js`, который содержит в себе весь JavaScript и CSS код. Подобный подход позволяет в значительной степени сократить время необходмое для сборки кода. Для запуска сервера можно воспользоваться npm скриптом, который запускает команду `node server.js` (посмотреть все доступные npm скрипты можно в файле `package.json` — свойство `scripts`):
```sh
npm start
```

Сборка проекта "в продакшен" отличается от той сборки, которой вы будете пользовать при разработке. Для проекта не генерируются сурсмэпы, происходит разделение JavaScript и CSS кода на два разных файла, весь код минифицируется.

### Автоматическая перезагрузка
Для автоматической перезагрузки страницы при изменении файлов используются:
* [Webpack Dev Server](https://webpack.github.io/docs/webpack-dev-server.html)
* [Hot Module Replacepent Plugin](https://webpack.github.io/docs/hot-module-replacement.html)
* [React Hot Loader](https://github.com/gaearon/react-hot-loader)

Все настройки для Webpack Dev Server можно найти в файле `server.js`: по умолчанию сервер работает по адресу `http://localhost:3000` и использует конфиг `webpack.config.js`.

### Сборка стилей
Сборка стилей в проекте осуществляется с помощью трёх лоадеров:
* [Style Loader](https://github.com/webpack/style-loader)
* [CSS Loader](https://github.com/webpack/css-loader)
* [Sass Loader](https://github.com/jtangelder/sass-loader)

Стандартная конфигурация Webpack предполагает, что все стили, написанные вами, будут включены в виртуальный файл `public/bundle.js`. При сборке проекта "в продакшен" стили будут выделены из JavaScript файла, скомпилированы, пропущены через [Autoprefixer](https://github.com/postcss/autoprefixer), минифицированы и соеденены в один файл `public/main.css`.

Выделением стилей из файла `bundle.js` осуществляется с помощью плагина для Webpack [Extract Text Plugin](https://github.com/webpack/extract-text-webpack-plugin).

#### Less и Stylus
Если вы не используете Sass, а вместо него предпочитаете [Less](http://lesscss.org/) или [Stylus](http://stylus-lang.com/), то вы можете исправить конфиги для Webpack. Сделать это достаточно просто.

Установите соответствующие лоадеры:
* [Stylus loader](https://github.com/shama/stylus-loader) — `npm install stylus-loader stylus --save-dev` (необходимо также установить сам Stylus)
* [Less loader](https://github.com/webpack/less-loader) — `npm install less-loader --save-dev`

После установки нужного лоадера необходимо указать Webpack'у, что нужно использовать именно их. Для этого потребуется исправить файлы `webpack.config.js` и `Webpack.config.prod.js`.

В файле `webpack.config.js` замените `sass` на `less` или `stylus` и поменяйте расширение в свойстве `test`:
```js
module: {
  loaders: [
    // Пример для Stylus
    {
      test: /\.styl$/, // меняем расширение файла
      loaders: ['style', 'css', 'stylus'] // и лоадер
    },

    // Пример для Less
    {
      test: /\.less$/, // меняем расширение файла
      loaders: ['style', 'css', 'less'] // и лоадер
    }
  ]
}
```

## Публикация проекта
Проект возможно опубликовать совершенно бесплатно, без регистрации и смс. Созданное вами приложение будет размещено на [Github Pages](https://pages.github.com/) и будет доступно по адресу `http://<account>.github.io/react-challenge-colorizr/`, где `<account>` соответствеует вашему никнейму на гитхабе, например, `http://rtivital.github.io/react-challenge-colorizr/`. Для автоматизации процесса публикации используется модуль [gh-pages](https://www.npmjs.com/package/gh-pages). Все настройки публикации вы сможете найти в файле `utils/deploy.js`.

Опубликовать проект можно с помощью npm скрипта:
```sh
npm run deploy
```
Скрипт запустит последовательно две задачи: сборку проекта командой `webpack --config webpack.config.prod.js` и публикацию командой `node utils/deploy.js`.
