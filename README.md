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

## Demo

``` bash
# install dependencies
yarn install

# serve with hot reload at localhost:8080
yarn run dev
