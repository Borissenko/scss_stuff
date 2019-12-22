https://vue-loader.vuejs.org/guide/pre-processors.html#sass
1. Инстилляция зависимостей
npm install sass-loader node-sass style-loader --save-dev
npm install sass-loader node-sass vue-style-loader --save-dev  ?
npm install sass-loader node-sass --save-dev  ?

2. webpack.base.conf.js
resolve: {
 extensions: ['.js', '.vue', '.json', '.scss'],
 alias: {
   'vue$': 'vue/dist/vue.esm.js',
   '@': resolve('src'),
   styles: resolve('src/assets/scss') //где расположены scss-файлы
 }
},
{
      test: /\.scss$/,  //в компоненте указываем <style lang="scss" scoped>
      use: [
        'style-loader',
        MiniCssExtractPlugin.loader,
        {
          loader: 'css-loader',
          options: { sourceMap: true }
        }, {
          loader: 'postcss-loader',
          options: { sourceMap: true, config: { path: `./postcss.config.js` } }
        }, {
          loader: 'sass-loader',
          options: { sourceMap: true }
        }
      ]
},


or
            {
                test: /\.scss$/,
                use: [
                    isDev ? 'vue-style-loader' : MiniCSSExtractPlugin.loader,
                    { loader: 'css-loader', options: { sourceMap: isDev } },
                    { loader: 'sass-loader', options: { sourceMap: isDev } }
                ]
            },
            {
                test: /\.sass$/,
                use: [
                    isDev ? 'vue-style-loader' : MiniCSSExtractPlugin.loader,
                    { loader: 'css-loader', options: { sourceMap: isDev } },
                    { loader: 'sass-loader', options: { sourceMap: isDev } }
                ]
            }

3. Подключение к проекту
//component.vue
<style lang="scss" scoped>        //внутри теперь можно использовать scss-код
 @import '../assets/scss/main';   //импорт main.scss
  
    .magic {
        color: $mainFontColor;
    }
</style>

or make import in main.js by
import './assets/scss/main.scss'
import './assets/css/main.css'


4. Получение SCSS-variables to component's JS.
https://www.bluematador.com/blog/how-to-share-variables-between-js-and-sass

a) В SCSS-стилях обозначаем :export 
$animation-ms : 20ms;
:export {
  animationHowLong: $animation-ms;
}

b) В компоненте используем
import styles from '../styles/animation.scss'

const mls = parseInt(styles.animationHowLong)
или
    computed: {
      ff() {
        return parseInt(styles.animationHowLong)
      }
    }


