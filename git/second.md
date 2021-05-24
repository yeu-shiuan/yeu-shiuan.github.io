# Javascript 筆記
youtube影片：https://www.youtube.com/watch?v=8aGhZQkoFbQ
常見術語："event-loop"、"non-blocking"、"callback"、"asynchronous"、"single-threaded"以及"concurrency"。

# 

### **Javascript 的執行環境**
* Javascript是單執行緒(single threaded)的方式進行。
* 分為全域和區域函式，全域函式在一開始進入時建立執行，區域函式在被呼叫時執行。
* 單緒執行(single threaded)，一次只執行一個指令或函式，每個函式被呼叫時會停止當前的執行環境，建立屬於該函式的區域執行環境，直到執行完執行環境會被移除，回到原本呼叫該函式的執行環境。


### **Call Stack (呼叫堆疊)**
* 紀錄著每個function執行時需要用到的資源，以及記錄function執行的順序。
* 依照先後順序做堆疊，先來後出，後來先出的順序。

### **blocking (阻塞)**
* 當執行需要等待很長的時間，或有卡住的現象，被稱為blocking(阻塞)。
* 如果是同步的情形，每次request必須先執行完，才能從stack(堆疊)中跳離開往下執行;而在瀏覽器上阻塞會造成瀏覽器的停滯，stack blocking 是指stack中未處理完的函式導致瀏覽器無法做渲染，必須等到request執行完瀏覽器才可繼續運作。

* *Render(渲染)解釋->又稱彩現、算繪、演繹，是指透過電腦程式製作出模型影像的一道程序。*

### **synchronous (同步) & asynchronous (非同步)**
* 同步（synchronous）代表執行時程式會卡在那一行，直到有結果為止。
* 非同步（asynchronous）代表執行時不會卡住，但執行結果不會放在回傳值，而是需要透過回呼函式（callback function）來接收結果。

### **event loop 事件循環**
* 瀏覽器中可以同時處理多件事情，是因為有多個Runtime(執行時期)，Runtime(執行時期)一次只做一件事，但瀏覽器提供不同的API讓我們可以搭配event loop非同步的同時處理多件事。

![](https://i.imgur.com/FMSnh6T.gif)

* event loop的作用是監控堆疊區(call stack)和工作佇列（task queue），當推疊區為空時，會把工作佇列中的內容拉到堆疊區作執行。

### **setTimeout 計時**
![](https://i.imgur.com/ErfP43I.png)
* 常見的JavaScript執行環境是瀏覽器和Node.js，而JavaScript需要搭配執行環境提供，例如：setTiemout。
* 影片中提及的setTimeout 0，是即使使用0秒，它一樣會先將函式放到WebAPIs的計時器中，當時間到時該函式放到工作佇列（task queue）內，到所有堆疊的內容都被清空後才會執行這個，和setTimeout的設定有關，與秒數無相關性。
* setTimeout只能保證在幾毫秒後會執行，並不是立即執行。

# 

### **影片中的其他例子**
#### 影片18:26
* click event->透過setTimeout的時間到時，或click事件被觸發時，WebAPIs會將它們放置到工作佇列（task queue）中，當推疊清空時，event loop就會將它搬上去，最後執行DOM。

#### 影片22:25
* 模擬器Render(渲染)的情況->渲染優先權高於回呼函式，當堆疊(stack)中執行一個耗時的函式，渲染會被阻塞，如果透過非同步的方式執行，每一個函式從工作佇列（task queue）到堆疊(stack)，提供了瀏覽器重新渲染的機會。

#### 影片24:47
* DOM的卷軸事件觸發情況->雖然不會造成堆疊(stack)的阻塞，但可能會造成工作佇列（task queue）的阻塞。