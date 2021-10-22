# УСТАНОВКА SCSS на Vue, КРАТКО
(по Dart-scss варианту подключения scss)

В последний раз установил Vue-болванку по Dart-scss варианту,
добавил vue.config.js, а

npm i sass sass-loader -D
npm i style-resources-loader -D

НЕ устанавливал - И ВСЕ РАБОТАЕТ(!).


...........................
# 1. Можно НЕ устанавливать уже!
npm i sass sass-loader -D            //не нужно, если установили Dart-scss в ходе Vue_create_project
npm i style-resources-loader -D      //для подключения констант

# 2. Подключаем импорт переменных
> Создаем в корне проекта vue.config.js
//vue.config.js

module.exports = {
  css: {
    loaderOptions: {
      scss: {
        prependData: `@import "~@/assets/main.scss";`  // ";" дб в конце(!). Use "~@/" одновременно(!).
      },
      // css: {
        // options here will be passed to css-loader
      // },
      // postcss: {
        // options here will be passed to postcss-loader
      // }
    }
  }
}


ИЛИ
module.exports = {
  css: {
    loaderOptions: {
      // по умолчанию опция `sass` будет применяться к обоим синтаксисам- и к sass, и к scss, и поэтому опцию scss можно не заявлять.
      sass: {
        prependData: `@import "~@/variables.scss";`   //(для scss- ";" в конце(!))
      },
      // css: {
        // options here will be passed to css-loader
      // },
      // postcss: {
        // options here will be passed to postcss-loader
      // }
    }
  }
};


