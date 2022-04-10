# youbikelocator

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


Usage
在區域空白欄位選擇欲查詢之區域，或者於後方欄位輸入地址或者道路名，接著按下 Search 鍵查詢。

搜尋結果將顯示在頁面下部。

點擊搜尋結果可開啟 google map 分頁顯示站點位置。

點擊 clear 鍵清除欄位中選擇或填寫的內容。



據輸入的區域或者地址搜尋站點位置，並取得空置 YouBike 車輛之即時資訊。優先搜尋區域，再搜尋部分字詞相符的地址。

使用 axios 引入數據，以 javascript 原生語法 filter 篩選其中區域與地址相符的項目並渲染至搜尋結果。
