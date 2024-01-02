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
#### latest
#### search
#### watch
#### detail

### 額外設定(選擇性)
        async load(){
        // 當插件載入時調用
        }
        async createFilter(filter){
        // 建立篩選器
        }
#### request
### Regex
### CSS 選擇器
## 資源與工具
+ #### Regex
  + [Regex101](https://regex101.com/) 用來測試Regex的好網站
