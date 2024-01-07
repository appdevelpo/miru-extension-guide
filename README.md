# web scraping guide(施工中)
**本文是針對如何爬取網站資料並放入使用 Miru app 提供的API 製作成插件**

**API 詳細內容請參閱 https://miru.js.org/developer/**

**For english guide please check [here](https://github.com/appdevelpo/web_scraping_guide/blob/main/english.md)**

# 基礎
## 插件編寫
會基本JavaScript即可，如果不會JS建議去網路上查閱更詳細的資訊
### 插件訊息
    // ==MiruExtension==
    // @name  Extension Name (插件名稱)
    // @version v0.0.1(版本號)
    // @author Xxxx (作者)
    // @lang zh-cn (語言)
    // @license      MIT
    // @nsfw         false (Boolean)
    // @icon https://xxx.xxx.xxx/xxx.png (favicon)
    // @package xxx.xxx.xxx (插件名稱)
    // @type bangumi (類型)
    // @webSite https://xxx.xxx.xxx/ (使用網站)
    // @description extension description (描述)
    // ==/MiruExtension==
##### 類型說明
|     @type     |                  說明           |
| ------------- | ------------------------------ |
| `manga`      | 漫畫       |
| `bangumi`   | 影片  ***只要是影片類的(動漫、電影等等)都是bangumi***  |
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
    // 觀看，點入小集後調用
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
對應不同種類(@type)返回不同數據
+ #### bangumi
    

      {
              type:String, // 串流類型 只有 "hls" "mp4" "torrent" 三種
              url:String,// 鏈結
              subtitles:[{
                  title:String //字幕標題
                  url:String   //字幕連結
              },...],
              audioTrack:String, //音频链接
              headers:{} //看需求加
          }
+ #### manga
        {
          urls: [], //圖片連結
          headers:{} //看需求加
          }
+ #### fikushon
        {
            content :[], //文字內容
            title:String,
            headers:{} //看需求加
        }
#### detail
        {
            title:String,//標題
            cover:String,//封面連結
            desc:String,//描述
            episodes: [{
                title: String,//子標題
                urls: {
                    name: String,//名稱
                    url: String//小集連結
                }
            },...]
        }
#### request
發送請求

        request(urltail,{
            headers:{
                "Miru-Url":urlhead
            }
            
        }//選擇性使用
        )
所發送出來的Url為 `urlhead + urltail`，如果 `"Miru-Url"` 沒有宣告app會自動將其設定為  `@website`
### Regex
基本上 Regex 的使用方法是用JavaScript的原生寫法，個人常用[`match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)和[`replace`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)，
### CSS 選擇器

#### querySelector
    querySelector(document,selector)
跟原本的`document.querySelector` 不同
#### querySelectorAll
    querySelectorAll(document,selector)
#### getAttributeText
    getAttributeText(document,seletor,attribute)
## 資源與工具
### 參考資源
[miru repo](https://github.com/miru-project/repo) miru 的官方的插件倉庫可以參考
  
下列只是個人常用的工具分享
+ ### Regex
    + [Regex101](https://regex101.com/)
      
        + 用來測試Regex的好網站
+ ### CSS 選擇器
    + [Ublock Orgion](https://github.com/gorhill/uBlock)
      
        + 雖說是廣告攔截器但內建 CSS 內建選擇器，只要點擊網頁介面CSS路徑就會出現
        + 內建紀錄器會記錄到訪網站的請求以及檔案(這對anti-debugging 非常有用，如果連devtool都被阻擋的話)
        + 一鍵關閉停用JavaScript
+ ### 網頁API 測試
    + PostMan
+ ### 對付anti debugger 的工具
    + Resource Override 
