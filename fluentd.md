# fluentd
一個分送資料流的中介平台，可以預處理數據後，分送到各自的儲存點（資料庫）

在k8s中，可以從收集從不同pod流出的stdout及stderr, 集中統一到fluend, 使用共同的pipeline來搜集數據

優點：log集中處理，方便維護


## usage
最常見的方式就是 source 收集日誌，然後由串聯的 filter 做流式的處理，最後交給 match 進行分發

- Source: 所有數據來自哪裡
	- http: 以http的port 收http的訊息
	- forward： 以tcp的port 收tcp的數據包
	- 可以同時進行
- Match: 分發日誌
	- 最好精確匹配
	- 順序很重要
- Filter:對data做加工
	- 一定會有@type: 要加什麼工, e.g:
		- record_transformer: 插入新欄位
		- teams: teams的plugin
		- copy: 複製ｄａｔａ
			- 可以接多個story
	- 有多個plugin可以參考

## Reference
- https://www.itread01.com/content/1549792272.html