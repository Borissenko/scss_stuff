install SCSS for Nuxt

# A.
npm install sass sass-loader@10 fibers --save-dev
npm install @nuxtjs/style-resources --save-dev

//nuxt.config.js
css: [
'~/assets/SCSS/main.scss'
],
styleResources: {
scss: ['~/assets/SCSS/main.scss']
},
buildModules: [
'@nuxtjs/style-resources',
],

// assets/SCSS/main.scss
@import 'constants';
@import 'utils';


// assets/SCSS/constants.scss
$red: #ec0505;



