# project

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).

Игра "Тренируй память":
В таблице 5x6 случайным образом размещаются карточки с картинками. 
Каждая в двух экземплярах. В начале игры все картинки видимы короткое время. 
Затем все карточки переворачиваются. При нажатии на карточку появляется картинка. 
Задача угадать позиции двух одинаковых картинок. 
Если пара не угадывается, то открытые карточки закрываются. Игра завершается когда открыты все пары.

Вся работа приложения должна происходить без перезагрузки страницы.

Реализавать на Vue.js