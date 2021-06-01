# **5/29-5/30 node.js 筆記+心得**
**📢 總覽：**
* 內容
1. xhr XMLHttpRequest ( 實作-同步和非同步 )
2. 補充：CORS-同源政策
3. axios 用法
4. 忽略檔案 .gitignore
5. 安裝模組( module )與版本控制
6. 模組( module ) / 套件( package )的使用
7. callback 與 Promise
8. Aysnc 與 Await
9. 後記心得

---


## **xhr XMLHttpRequest**

* 是最常見的 JavaScript HTTP Client，常見於 Web 應用、Debug、API 測試…。
* 能透過它操作 HTTP 請求，進行網路作業，擷取資料的同時，卻不需進行頁面重載( page reload )，增加 Web 效能與體驗，這種**非同步**的 Web 應用架構，稱為 **AJAX**。
* XMLHttpRequest ( XHR ) 已無法應對現今複雜的 Web 環境，容易陷入波動拳( Callback Hell ) 。

* 參考資料：<https://developer.mozilla.org/zh-TW/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest>

**實例：設定請求**
```javascript=
<html>
  <head>
    <meta charset="UTF-8" />
    <title>XHR</title>
  </head>
  <body>
    <button id="syncBtn">同步</button>
    <button id="asyncBtn">非同步</button>
    <button id="countBtn">計數器</button>
    <div id="count">0</div>
    <div id="message">XXXXX</div>
  </body>
  <script>
    var message = document.getElementById("message");
    var syncBtn = document.getElementById("syncBtn");
    var asyncBtn = document.getElementById("asyncBtn");
    var countBtn = document.getElementById("countBtn");
    var count = document.getElementById("count");

    countBtn.addEventListener("click", function () {
      count.innerText = parseInt(count.innerText, 10) + 1;
    });
  </script>
</html>
```
* HTTP 請求方式( method ):GET、POST...
* 建立請求取得資料的方式可以為非同步( asynchronously ) 或 同步( synchronously )兩種之一，async 參數為 true 或是未指定設定為非同步，相反為false則會被設定為同步。
 
**非同步請求**
```javascript=
asyncBtn.addEventListener("click",function(){
        var xhr = new XMLHttpRequest();
        xhr.open("GET","http://34.217.120.25:3000", true);
        xhr.onload = function (){
        message.innerText = `非同步請求load ${this.responseText}`;
      };
      xhr.send();
    });
```
**同步請求**
```javascript=
syncBtn.addEventListener("click",function(){
        var xhr = new XMLHttpRequest();
        xhr.open("GET","http://34.217.120.25:3000", false);
        xhr.onload = function (){
        message.innerText = `同步請求load ${this.responseText}`;
      };
      xhr.send();
    });
```
<br><br/>
## **補充-跨來源資源共用（Cross-Origin Resource Sharing (CORS)）**
針對不同源的請求而定的規範，透過 JavaScript 存取非同源資源時，server必須明確告知瀏覽器允許何種請求，只有server允許的請求能夠被瀏覽器實際發送，否則會失敗。

這樣的設計是為了防範駭客的攻擊，在正常的情況下，駭客就不能夠任意用一個惡意的網站，去呼叫網路的服務。

**同源政策 ( Same-Origin Policy )**
需滿足下面幾點：
1. 相同的通訊協定 ( protocol )，即 http/https
2. 相同的網域 ( domain )
3. 相同的通訊埠 ( port )

* 參考資料：<https://medium.com/%E7%A8%8B%E5%BC%8F%E7%8C%BF%E5%90%83%E9%A6%99%E8%95%89/same-origin-policy-%E5%90%8C%E6%BA%90%E6%94%BF%E7%AD%96-%E4%B8%80%E5%88%87%E5%AE%89%E5%85%A8%E7%9A%84%E5%9F%BA%E7%A4%8E-36432565a226>
<br><br/>

## **asios 基本使用**
**安裝 GET / POST 基礎用法**
```javascript=
npm install axios
yarn add axios
```
**asios 組成結構**
```javascript=
axios.(config 物件)
.then(function (response) {})
.catch(function (error) {});
```
**then 時常用的回應參數**
```javascript=
axios.get('/users/123')
.then(function(response) {
    console.log(response.data); // api回傳的資料
    console.log(response.status); // HTTP狀態碼
    console.log(response.config); // Request的config(物件)
});
```
<br><br/>
## **使用 .gitignore 忽略檔案**

忽略檔案：
不應與其他開發人員共享的本地配置檔案
包含機密資訊的檔案，例如登入密碼，金鑰和憑據

**其他指令：**
`-f 參數f是強制加入的意思`
```bash=
$ git add -f 檔案名稱 //忽略.gitignore的規則
$ git clean -fX //清除忽略的檔案
```
* 參考資料：<http://www.tastones.com/zh-tw/stackoverflow/git/ignoring-files-and-folders/ignoring_files_and_directories_with_a_.gitignore_file/>
<br><br/>

## **安裝模組( module )與版本控制**

下載版本 -> npm install 

全部版本檢查 -> npm view cowsay versions

最新版本 -> npm view cowsay version

特定版本 -> npm i cowsay@1.1.3

更新到最新版 -> npm update

降版 -> npm install cowsay@1.3.0

使用 -> npx cowsay

移除 -> npm uninstall cowsay

安裝到全域 -> npm install -g cowsay


 **安裝至全域依照文件**
 ```bash=
 npm install -g @vue/cli
 vue create my-project
 ```
 **或**
 ```bash=
 npx @vue/cli create my-project
 ```
```
 透過npx:
 1. 執行本機的命令 （不管是全域的或專案下）
 2. 不用安裝命令，就能利用 npx 來執行
```
![](https://i.imgur.com/oeLX2Tk.jpg)
#### ( Σ(T□T) 恩 ... cowsay應該不太會用到的東西！重點是觀念！)
<br><br/>
## **模組( module ) / 套件( package )的使用**

require 載入模組
```
var express=require('express');
```
讀檔
```
fs.readFile(fileName [,options], callback)
```
檔案系統操作
<table>
  <tr>
    <td> Method方法</td>
    <td>Description描述</td>
  </tr>
  <tr>
    <td>fs.readFile(fileName [,options], callback)</td>
    <td>讀取現有文件</td>
  </tr>
  <tr>
    <td>fs.writeFile(filename, data[, options], callback)</td>
    <td>寫入文件，果存在則覆蓋，否則建立一個新的文件。</td>
  </tr>
  <tr>
    <td>fs.open(path, flags[, mode], callback)</td>
    <td>打開文件進行閱讀或寫入。</td>
  </tr>
  <tr>
    <td>fs.rename(oldPath, newPath, callback)</td>
    <td>重命名現有文件</td>
  </tr>
  <tr>
    <td>fs.stat(path, callback)</td>
    <td>返回包含重要文件統計信息的fs.stat 物件。</td>
  </tr>
  <tr>
    <td>fs.rmdir(path, callback)</td>
    <td>重命名現有目錄。</td>
  </tr>
  <tr>
    <td>fs.mkdir(path[, mode], callback)</td>
    <td>創建新目錄。</td>
  </tr>
  <tr>
    <td>fs.readdir(path, callback)</td>
    <td>讀取指定目錄的內容。</td>
  </tr>
   <tr>
    <td>fs.utimes(path, atime, mtime, callback)</td>
    <td>更改文件的時間標記。</td>
  </tr>
   <tr>
    <td>fs.exists(path, callback)</td>
    <td>確定指定的文件是否存在。</td>
  </tr>
   <tr>
    <td>fs.access(path[, mode], callback)</td>
    <td>測試指定文件的用戶權限。</td>
  </tr>
   <tr>
    <td>fs.appendFile(file, data[, options], callback)</td>
    <td>將新內容附加到現有文件。</td>
  </tr>
</table>

參考資料：<https://nodejs.org/api/fs.html> ( 官網 ) 

<https://ithelp.ithome.com.tw/articles/10185422> ( Node.js 檔案系統 )

<http://www.tastones.com/zh-tw/tutorial/nodejs/nodejs-modules-create-publish/> ( Node.js 建立、釋出、擴充套件和管理 )
<br><br/>

## **callback 與 Promise**
### **Callback Function**
Callback function 是一個被作為參數帶入另一個函式中的「函式」，這個被作為參數帶入的函式將在「未來某個時間點」被呼叫和執行。

### **callback hell (回呼地獄)**

![](https://i.imgur.com/kwuaIL1.png)
```
👎 缺點：
1. 可讀性低：如果程式碼出錯，要回頭慢慢找錯誤的地方
2. 可維護性低：如果要修改其中一組函式，牽一髮而動全身
```
### **為了解決這個問題，於是誕生了 Promise!!!**

### **Promise ⚡⚡**
### Promise 是一個表示非同步運算的最終完成或失敗的物件
Promise 是一個物件，代表著一個尚未完成，但最終會完成的一個動作，在一個「非同步處理」的流程中，它只是一個暫存的值（ Placeholder ）。
Callback 以外的另一種方式來處理非同步事件，且可讀性與可維護性比 Callback 好很多。
![](https://i.imgur.com/GWS9Z4e.png)
### ``Promise → pending → ( resolve or reject )``
* pending：事件已經運行中，尚未取得結果。
* resolved：事件已經執行完畢且成功操作，回傳 resolve 的結果（ 該承諾已經被實現 fulfilled ）。
* rejected：事件已經執行完畢但操作失敗，回傳 rejected 的結果。

**語法：**
```
new Promise( /* executor */ function(resolve, reject) { ... } );
```

```javascript=
// 非同步的作業
const getData = new Promise((resolve, reject) => {
  
  // 作業完成，並回傳錯誤訊息時
  if (error) { return reject('錯誤訊息')} 
  
  // 作業成功完成
  resolve({
    data: '成功就印出',
  })
})
```

### **then & catch**
* then() 方法：當成功從 resolve() 獲得結果時 -> 狀態由Pending轉為Fulfilled->then() 方法就會被調用。
* catch() 方法：當失敗從 reject() 獲得錯誤訊息時 -> 狀態由Pending轉為Rejected->catch() 方法就會被調用來處理錯誤。
```javascript=
getData
  // 使用then方法，並將成功訊息印出來
  .then(data => {console.log('成功資料', data)})
  // 使用catch方法，並將錯誤訊息印出來
  .catch(error => console.log('錯誤訊息', error))
```
### **Finally**
* 最後使用 finally 來確認工作結束，Promise最後的狀態如何都會執行，透過 finally 來關閉讀取的效果。
```javascript=
getData
  .then(success => {
    console.log(success);
  }).finally(() => {
    console.log('done');
  })
```

### **Promise Chain (串連 then)**
* Promise 解決 callback hell 用 then() 串接， then() 方法回傳的是一個「新的 promise」，以此往下互相串接
```javascript=
  // Promise chain
  // 使用 then 方法，並將成功訊息印出來
  doWorkPromise("刷牙", 2000, true)
    .then((result) => {
      // fulfilled 處理成功 resolve
      console.log(result);
      return doWorkPromise("吃早餐", 3000, false);
    })
    // 獲得前一個 then() 回傳的結果
    .then((result) => {
      console.log(result);
      return doWorkPromise("寫功課", 5000, true);
    })
    // 獲得前一個 then() 回傳的結果
    .then((result) => {
      console.log(result);
    })
    // 使用catch方法，並將錯誤訊息印出來
    .catch((err) => {
      // rejected 處理失敗 reject
      console.error("發生錯誤", err);
    })
    .finally(() => {
      console.log("我是 Finally");
    });
```
### **Promise 進階**
* `all`-> 多個 Promise 行為同時執行，全部完成後統一回傳，以陣列的形式傳入多個 promise 函式，在全部執行完成後回傳陣列結果，陣列結果順序與一開始傳入的一致。
```javascript=
Promise.all([promise(1), promise(2), promise(3, 3000)])
  .then(res => {
    console.log(res);
  });
```
* `race` -> 多個 Promise 同時執行，但僅回傳第一個完成的。
```javascript=
Promise.race([promise(1), promise(2), promise(3, 3000)]).then(res => {
  console.log(res);
});
```

## **Aysnc 與 Await**
### **Aysnc / Await**
**語法糖：**
* 是一種新的語法撰寫方式，來處理「非同步事件」。
* 非同步的程式碼讀起來像是「同步程式碼」。
* 是用來簡單化 Promise Chain 相對複雜的結構。
* await 只能在 async function 裡，async function 回傳是 Promise 物件，只是針對 promise-based 寫法進行包裝。

**語法：**
```javascript=
async function asyncFn() {
  return 'a';
}
console.log(asyncFn());
```
await 讓 async 函式的執行動作暫停，等到它獲得回傳的 Promise 物件後--無論執行成功（fulfilled）或失敗（rejected），才恢復執行 async 函式。

### **錯誤處理**
當獲得錯誤回傳（Rejected）時，await 會丟出 reject 的值，可以使用 try…catch…來捕捉錯誤原因。

```javascript=
async function asyncFunc() {
  try {
    const result1 = await experss1();
    console.log(result1);
    const result2 = await express2();
    console.log(result2);
  } catch (error) {
    console.log(error.message);
  }
}
```
```
👍 優點：
1. 可以直接將 await 獲得的回傳值存於一個變數中做後續使用，不是再呼叫 then() 方法串接。
2. 程式碼看起來更像在處理「同步程式碼」，提升了易讀性與可維護性。
```
參考資料：<https://realdennis.medium.com/callback-hell-%E8%88%87-promise-%E4%B8%80%E8%B5%B7%E4%BE%86%E6%8A%8A-settimeout-%E5%B0%81%E8%A3%9D%E6%88%90-promise-%E5%90%A7-e542ef84967f>

<https://medium.com/%E9%BA%A5%E5%85%8B%E7%9A%84%E5%8D%8A%E8%B7%AF%E5%87%BA%E5%AE%B6%E7%AD%86%E8%A8%98/%E5%BF%83%E5%BE%97-%E8%AA%8D%E8%AD%98%E5%90%8C%E6%AD%A5%E8%88%87%E9%9D%9E%E5%90%8C%E6%AD%A5-callback-promise-async-await-640ea491ea64>

<https://wcc723.github.io/development/2020/02/16/all-new-promise/>

<https://noob.tw/js-async/>

### **Promise 你是神你是唯一的神話**
![](https://i.imgur.com/a47aHnI.jpg)

圖片來源：
<https://realdennis.medium.com/javascript-%E5%85%A9%E7%A8%AE%E6%96%B9%E6%B3%95%E8%AE%93promise%E6%8C%89%E7%85%A7%E9%A0%86%E5%BA%8F%E5%9F%B7%E8%A1%8C-mergepromise-cc2d5903419>

### **而不會用的我是唯一的傻瓜**
![](https://i.imgur.com/OKrsBv7.png)

## **後記心得**

終於寫到後記，這次寫完筆記有種如釋重負的感覺，雖然第一次看到promise覺得頭很痛，甚至上課拼拼湊湊的爬蟲程式居然可以跑得動，當下超驚訝，同學還密我怎麼寫的，我也不知道啊！！這兩天再重新看過上課影片和相關文章後，好像有那麼一點點了解，但頭還是很痛，有種真得下去寫我可能還是寫不太出來的感覺，哭啊!!!看來只能努力再努力了!我的山很高~
(´−｀) ﾝｰ </font>
