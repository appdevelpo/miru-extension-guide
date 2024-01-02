# web scraping guide(施工中)
**本文是針對如何爬取網站資料並放入使用 Miru app 提供的API 製作成插件**

**API 詳細內容請參閱 https://miru.js.org/developer/**

# 基礎
## 插件編寫
會基本JavaScript即可，如果不會JS建議去網路上查閱更詳細的資訊，這裡除了介紹如何使用編寫插件也會提供一些常用的Regex 和CSS選擇器的寫法
### 插件訊息
    // ==MiruExtension==
    // @name  Extension Name(插件名稱)
    // @version v0.0.1(版本號)
    // @author Xxxx (作者)
    // @lang zh-cn (語言)
    // @license      MIT
    // @nsfw         false
    // @icon https://xxx.xxx.xxx/xxx.png (favicon)
    // @package xxx.xxx.xxx (插件名稱)
    // @type bangumi (類型)
    // @webSite https://xxx.xxx.xxx/ (使用網站)
    // ==/MiruExtension==
##### 類型說明
|     @type     |                  說明           |
| ------------- | ------------------------------ |
| `manga`      | 漫畫       |
| `bangumi`   | 影片     |
| `fikushon`|       小說 |
### 基本框架
    export default class extends Extension {
      async latest(page) {
    // 最近更新
      }
      async search(kw,page,filter) {
    // 搜尋、當app資料滑到底部時調用、當用戶使用篩選器並按確定時調用
      }
      async detail(url) {
    // 詳細資訊
      }
      async watch(url) {
    // 觀看
      }
    }



### 額外設定(選擇性)
    async load(){
    // 當插件載入時調用
    }
    async createFilter(filter){
    // 建立篩選器
    }
### 返回格式
#### latest
    [{
        title: String, // 標題
        cover: String, //封面連結
        url:String, //給detail時的url
        update:String, 更新集數
    },...]
#### search
    [{
        title: String, // 標題
        cover: String, //封面連結
        url:String, //給detail時的url
        update:String, 更新集數
    },...]
#### watch
+ ##### bangumi
    +     {
              type:String, // 串流類型 只有 "hls" "mp4" "torrent" 三種
              url:String,// 鏈結
              subtitles:[{
                  title:String //字幕標題
                  url:String   //字幕連結
              },...]
          }
#### detail
#### request
### Regex
### CSS 選擇器
## 資源與工具

這些只是個人常用的工具分享，不安裝甚麼事都不會發生
+ ### Regex
    + [Regex101](https://regex101.com/)
      
        + 用來測試Regex的好網站
+ ### CSS 選擇器
    + [Ublock Orgion](https://github.com/gorhill/uBlock)
      
        + 雖說是廣告攔截器但內建 CSS 內建選擇器，只要點擊網頁介面CSS路徑就會出現
        + 內建紀錄器會記錄到訪網站的請求以及檔案(這對anti-debugging 非常有用，如果你連devtool都被阻擋的話)
        + 一鍵關閉停用JavaScript
+ ### 網頁API 測試
    + Fiddler
    + PostMan
+ ### 對付anti debugger 的工具
    + Resource Override 
