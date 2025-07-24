# GeoGebra HTML 架構代碼詳細解析

## 1. HTML 基本結構

### DOCTYPE 和 HTML 標籤
```html
<!DOCTYPE html>
<html>
```
- 定義為 HTML5 文檔
- 設定 HTML 根元素

### Head 區段
```html
<head>
<meta name=viewport content="width=device-width,initial-scale=1">
<meta charset="utf-8"/>
<script src="https://www.geogebra.org/apps/deployggb.js"></script>
</head>
```
- `viewport`：設定響應式設計，適應不同裝置螢幕
- `charset="utf-8"`：設定字元編碼為 UTF-8
- `deployggb.js`：載入 GeoGebra 部署腳本，核心函式庫

### Body 區段
```html
<body>
<div id="ggbApplet"></div>
```
- 創建一個 ID 為 "ggbApplet" 的容器，GeoGebra 小程式將注入此處

## 2. JavaScript 配置區段

### Parameters 物件 - 基本設定
```javascript
var parameters = {
"id": "ggbApplet",           // 小程式的唯一識別符
"width":1536,                // 寬度 1536 像素
"height":826,                // 高度 826 像素
```

### 介面顯示控制
```javascript
"showMenuBar":true,          // 顯示選單列
"showAlgebraInput":true,     // 顯示代數輸入列
"showToolBar":true,          // 顯示工具列
"showToolBarHelp":true,      // 顯示工具列說明
"showResetIcon":false,       // 不顯示重設圖示
"showZoomButtons":true,      // 顯示縮放按鈕
"showFullscreenButton":true, // 顯示全螢幕按鈕
```

### 自訂工具列
```javascript
"customToolBar":"0 | 1 501 5 19 , 67 | 2 15 45 18 , 7 37 | 514 3 9 , 13 44 , 47 | 16 51 | 551 550 11 ,  20 22 21 23 , 55 56 57 , 12 | 69 | 510 511 , 512 513 | 533 531 , 534 532 , 522 523 , 537 536 , 535 , 538 | 521 520 | 36 , 38 49 560 | 571 30 29 570 31 33 | 17 | 540 40 41 42 , 27 28 35 , 6 , 502",
```
**工具列編號對應：**
- `0`：移動工具
- `1`：點工具
- `2`：直線工具
- `67`：向量工具
- `|`：分隔符號，用於分組工具
- `,`：同組內工具分隔

### 互動設定
```javascript
"enableLabelDrags":false,     // 禁用標籤拖拽
"enableShiftDragZoom":true,   // 啟用 Shift+拖拽縮放
"enableRightClick":false,     // 禁用右鍵選單
"errorDialogsActive":false,   // 禁用錯誤對話框
"useBrowserForJS":false,      // 不使用瀏覽器 JavaScript 引擎
"allowStyleBar":false,        // 不允許樣式列
"preventFocus":false,         // 不防止焦點
```

### 縮放和顯示設定
```javascript
"scale":1,                    // 預設縮放比例 1:1
"disableAutoScale":false,     // 啟用自動縮放
"allowUpscale":false,         // 不允許放大
"clickToLoad":false,          // 不需點擊載入
"capturingThreshold":3,       // 捕捉閾值 3 像素
```

### 外觀設定
```javascript
"appName":"Linux",            // 應用程式名稱（注意：原檔案顯示"Linux"，應為"classic"）
"buttonRounding":0.7,         // 按鈕圓角 0.7
"buttonShadows":false,        // 不顯示按鈕陰影
"language":"en",              // 介面語言：英文
```

### 載入選項（註解掉的選項）
```javascript
// "material_id":"RHYH3UQ8",  // 從 geogebra.org 載入特定教材
// "filename":"myfile.ggb",   // 載入本地 .ggb 檔案
```

### 回調函數
```javascript
"appletOnLoad":function(api){ 
    /* api.evalCommand('Segment((1,2),(3,4))');*/ 
},
```
- 小程式載入完成後執行的函數
- `api` 參數提供 GeoGebra API 接口
- 範例：`api.evalCommand()` 可執行 GeoGebra 指令

## 3. 視圖配置

### Views 物件
```javascript
var views = {
    'is3D': 1,      // 啟用 3D 視圖
    'AV': 1,        // 啟用代數視圖 (Algebra View)
    'SV': 0,        // 停用試算表視圖 (Spreadsheet View)
    'CV': 0,        // 停用 CAS 視圖 (Computer Algebra System)
    'EV2': 0,       // 停用第二繪圖視圖 (Graphics View 2)
    'CP': 0,        // 停用作圖步驟 (Construction Protocol)
    'PC': 0,        // 停用機率計算器 (Probability Calculator)
    'DA': 0,        // 停用資料分析 (Data Analysis)
    'FI': 0,        // 停用函數檢查器 (Function Inspector)
    'macro': 0      // 停用巨集 (Macros)
};
```

## 4. 小程式初始化

### 創建和注入
```javascript
var applet = new GGBApplet(parameters, '5.0', views);
window.onload = function() {applet.inject('ggbApplet')};
```
- 創建 GGBApplet 實例，版本 5.0
- 頁面載入完成後將小程式注入到 "ggbApplet" 容器

### 預覽圖片設定
```javascript
applet.setPreviewImage(
    'data:image/gif;base64,R0lGODlhAQABAAAAADs=',           // 1x1 透明 GIF
    'https://www.geogebra.org/images/GeoGebra_loading.png',   // 載入中圖片
    'https://www.geogebra.org/images/applet_play.png'         // 播放按鈕圖片
);
```

## 總結

這個架構創建了一個功能完整的 GeoGebra 網頁應用程式，包含：
- 3D 和代數視圖
- 完整的自訂工具列
- 響應式設計
- 可程式化的 API 接口
- 適合教學和互動式數學內容展示