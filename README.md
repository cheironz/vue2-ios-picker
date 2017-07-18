# vue2-ios-picker

> iOS style picker component for vue2.0

Thanks for [picker](https://github.com/ustbhuangyi/picker) by [ustbhuangyi](https://github.com/ustbhuangyi)

## Props

``` javascript
themeColor: '#000000' // define the color of confirm button text
title: 'Picker' // display in center of picker head
items: [
            [
                {
                    text: 'display',
                    value: 'for code'
                },
                {
                    text: 'display',
                    value: 'for code'
                }
            ],
            [
                {
                    text: 'display',
                    value: 'for code'
                },
                {
                    text: 'display',
                    value: 'for code'
                }
            ]
        ]  // require
selectedIndex: [0, 2, 3] // index of default select, default: [0, 0, 0]
```

## Event
``` javascript
change // column stop scroll. params: column index, item index of this column
select // click confirm button. params: value array of all column, index array of all column
cancel // user close picker
value-change // click confirm button and this value is different from the previous one. params: value array of all column, index array of all column
```

## Methods

``` javascript
show(next) // next a function, callback after showing picker. NOT require
hide() // close picker
scrollColumn(index, dist) // index is the index of target column, dist is the index of item you want to scroll to.
```

## Demo

``` bash
# install dependencies
yarn install

# serve with hot reload at localhost:8080
yarn run dev
