---
layout: post
title:  "[架構設計] 目的 & 複雜度來源"
description: "此篇文章是極客時間\"從 0 開始學架構\"課程時所留下的學習筆記，主要內容為架構設計的目的 & 複雜度(高性能、高可用、高擴展性、低成本、安全、規模)來源"
date: 2020-08-02 21:15:00
published: true
comments: true
categories:
  - Architecture Design
tags:
  - Architecture Design
---


架構設計的目的
============

## 對架構設計常見的誤解

### 因為架構很重要，所以要做架構設計

- 其實不一定，一間公司的初期產品可能是沒有架構設計的，而一開始沒有架構設計時，反而開發效率是最高的，進行架構設計畢竟需要投入時間和人力

- 如果系統使用人數沒有很多，或是資料量不大，沒有架構設計也不一定會發生什麼問題

- 架構設計並無法保證可以促進業務發展

### 不是每個系統都要做架構設計嗎?

- 架構師很容易直接套用業界其他公司的架構，但卻沒仔細思考是否符合公司實際需求

- 不適合的系統架構可能會導致不斷重構、甚至砍掉重練

### 公司流程中要求系統開發過程中必須有架構設計

- 不要為了做架構設計而做

### 為了高性能、高可用、可擴展，所以要做架構設計

不經過審慎思考，直接就設計高性能、高可用、可擴展的架構，可能會出現以下問題：

- 系統初期就過於複雜

- 專案項目落地的時間遙遙無期

- 上線後運行不夠穩定，遇到問題不容易定位，很難解決


## 架構設計的真正目的

- 架構設計的主要目的是**為了解決軟體系統複雜度帶來的問題**
> 遵循這條準則能夠讓"新手"架構師心中有數，而不是一頭霧水。

- 架構設計的目的不是為了"防範"後期變化，而是要**適當預測然後做好準備**

以下舉例說明：
- 我們的系統一定要做到每秒 TPS 10 萬
> 如果系統的複雜度不是在性能這部分，TPS 做到 10 萬並沒有什麼用

- 淘寶的架構是這麼做的，我們也要這麼做
> 淘寶的架構是為瞭解決淘寶業務的複雜度而設計的，淘寶的業務複雜度並不就是我們的業務複雜度，絕大多數業務的用戶量都不可能有淘寶那麼大

- Docker 現在很流行，我們的架構應該將 Docker 應用進來
> Docker 並非萬能的，只是為瞭解決資源重用和動態分配而設計的，如果我們的系統複雜度根本不是在這方面，引入 Docker 沒有什麼意義


## 討論整理精華

- 架構是為了應對軟體系統複雜度而提出的一個解決方案

- 架構即(重要)決策，是在一個有約束的盒子裡去求解或接近最合適的解

- 這個有約束的盒子是團隊經驗、成本、資源、進度、業務所處階段等所編織、摻雜在一起的綜合體(人，財，物，時間，事情等)

- 架構無優劣，但是存在恰當的架構用在合適的軟體系統中，而這些就是決策的結果

- 需求驅動架構；在分析設計階段，需要考慮一定的人力與時間去"跳出代碼，總攬全局"，為業務和IT技術之間搭建一座"橋樑"

- 軟體架構是為兩件事服務的：`業務架構`和`業務量級`，業務架構和業務量級都是從每個具體項目的實際應用場景中提煉出來的

- 業務架構是對業務需求的提煉和抽象，開發軟體必須要滿足業務需求，否則就是空中樓閣。軟體系統業務上的複雜度問題，可以從業務架構的角度切分工作交界面來解決。設計軟體架構，首先是要保證能和業務架構對的上，這也是從業務邏輯轉向代碼邏輯的過程，所以軟體架構的設計為開發指明了方向。另外架構設計也為接下來的開發工作分工奠定了基礎。

- 業務量級表現在存儲能力、吞吐能力和容錯能力等，主要是軟體運維期業務的複雜度。做軟體架構設計，是要保證軟體有能力托起它在業務量級上的要求的，如果軟體到運行使用期廢了，前面所有的工作都付諸東流了。不同的業務量級，對應的軟體的架構複雜度是不同的，所以對於不同的項目，業務量級不同，架構設計也不同。

- 做業務架構必須與其面向的實際應用場景相匹配；所以每個軟體在開發前，都要結合自己的應用場景設計適合自身的軟體架構，現成的架構方案只能借鑑，不能直接套用

- 由於業務架構和業務量級也會不斷調整或長大，軟體架構也不是一勞永逸的，會隨業務不斷調整



複雜度來源：高性能(High Performance)
=================================

軟體系統中高性能帶來的複雜度主要體現在兩方面：

- 單機內部為了高性能帶來的複雜度

- 多機集群為了高性能帶來的複雜度


## 單機複雜度

- 將硬體性能充分發揮出來的關鍵就是 OS(Operating System)，所以 OS 本身其實也是跟隨硬體的發展而發展的， OS 是軟體系統的運行環境，OS的複雜度直接決定了軟體系統的複雜度

- OS 中和性能最相關的就是 `Process` 和 `Thread`

- 為了解決手工操作帶來的低效率，批次處理 OS 應運而生

- 為了進一步提升性能，人們發明了 `Process` 來對應一個任務，每個任務都有自己獨立的內存空間，process 間互不相關，由 OS 來進行調度

- 此時的 CPU 還沒有 multiple core 和 multiple thread 的概念，為了達到 multiple process 並行運行的目的，採取了分時的方式，即把 CPU 的時間分成很多片段，每個片段只能執行某個 process 中的指令

- 從用戶的角度來看，兩個任務之間能夠在運行過程中就進行通信，會讓任務設計變得更加靈活高效；為了解決這個問題，process 間通信的各種方式被設計出來了，包括 pipeline、message queue、signal、shared storage .... 等等

- 單個 process 內部只能串行處理，而實際上很多 process 內部的子任務並不要求是嚴格按照時間順序來執行的，也需要並行處理；為了解決這個問題，人們又發明了 thread，thread 是 process 內部的子任務，但這些子任務都共享同一份 process 數據。為了保證數據的正確性，又發明了 Mutex(互斥鎖)機制。

- 有了 multiple thread 後， **OS 調度的最小單位就變成了 thread**，而 **process 變成了 OS 分配資源的最小單位**

- multiple process, multiple thread 雖然讓多任務並行處理的性能大大提升，但本質上還是分時系統，並不能做到時間上真正的並行

- 如何達到真正並行處理? => 讓多個 CPU 能夠同時執行計算任務，從而實現真正意義上的多任務並行，以下是常見架構：
  - SMP（Symmetric Multi-Processor，對稱多處理器結構）
  - NUMA（Non-Uniform Memory Access，非一致存儲訪問結構）
  - MPP（Massive Parallel Processing，海量並行處理結構）


## Cluster(集群)的複雜度

透過大量機器來提升性能，並不僅僅是增加機器這麼簡單，讓多台機器配合起來達到高性能的目的，是一個複雜的任務


### 任務分配

- 任務分配的意思是指每台機器都可以處理完整的業務任務，不同的任務分配到不同的機器上執行

- 實際上"**任務**"涵蓋的範圍很廣，可以指完整的業務處理，也可以單指某個具體的任務。例如："存儲"、"運算"、"cache"..等都可以作為一項任務，因此存儲系統、運算系統、緩存系統都可以按照任務分配的方式來搭建架構。

- "任務分配器"也並不一定只能是物理上存在的機器或者一個獨立運行的程序，也可以是嵌入在其他程序中的算法，例如 Memcache 的集群架構

- 雖然系統拆分可能在某種程度上能提升業務處理性能，但提升性能也是有限的，因為**最終決定業務處理性能的還是業務邏輯本身，業務邏輯本身沒有發生大的變化下，理論上的性能是有一個上限的，系統拆分能夠讓性能逼近這個極限，但無法突破這個極限**


## 討論整理精華

- 性能是軟體的一個重要質量屬性。衡量軟體性能包括了響應時間、TPS、 server 資源利用率等客觀指標，也可以是用戶的主觀感受（從程式設計師、業務用戶、終端用戶/客戶不同的視角，可能會得出不同的結論）。

- 在說性能的時候，有一個概念與之緊密相關 => `伸縮性`，這是兩個有區別的概念
> 性能更多的是衡量軟體系統處理一個請求或執行一個任務需要耗費的時間長短；而伸縮性則更加關注軟體系統在不影響用戶體驗的前提下，能夠隨著請求數量或執行任務數量的增加（減少）而相應地擁有相適應的處理能力

- 什麼是**"高"性能**？這可能是一個動態概念，與當前的技術發展狀況與業務所處的階段緊密相關。比如，現在在行業/企業內部認為的高性能，站在5年後來看，未必是高性能
  - 站在架構師、設計師的角度，高性能需要和業務所處的階段來衡量。高到什麼程度才能與當前或可預見的未來業務增長相匹配。一味去追求絕對意義上的高，沒有太大的實際意義。因為，**伴隨性能越來越高，相應的方法和系統複雜度也是越來越高，而這可能會與當前團隊的人力、技術、資源等不相匹配**
  - 但是什麼才是合適的高性能? 這可能需要從國、內外的同行業規模相當、比自己強的競爭者、終端用戶使用反饋中獲取答案並不斷迭代發展

- 為什麼需要高性能？
  - 追求良好的用戶體驗
  - 滿足業務增長的需要

- 如何做好高性能？
> 可以從垂直與水平兩個維度來考慮。垂直維度主要是針對單台計算機，通過升級軟、硬體能力實現性能提升；水平維度則主要針對集群系統，利用合理的任務分配與任務分解實現性能的提升。

- 垂直維度可包括以下措施：
  - 增加 memory，減少 I/O 操作
  - 更換 SSD 提升 I/O 訪問速度
  - 使用 RAID 增加 I/O 吞吐能力
  - 更換 server 獲得更多的 CPU 或分配更多的 virtual core
  - 升級網路接口或增加網路接口

- 水平維度可包括以下措施：
  - 功能分解：基於功能將系統分解為更小的子系統
  - 多實例副本：同一元件重複部署到多台不同的 server 
  - 數據分割：在每台機器上都只部署一部分數據

- 垂直維度方案比較適合業務階段早期和成本可接受的階段，該方案是提升性能最簡單直接的方式，但是**受成本與硬體能力天花板的限制**

- 水平維度方案所帶來的好處要在業務發展的後期才能體現出來；此方案會花費更多的硬體成本，另外一方面對技術團隊也提出了更高的要求；但是沒有垂直方案的天花板問題
  - 一旦達到一定的業務階段，水平維度是技術發展的必由之路
  - 作為技術部門，需要提前佈局 ，未雨綢繆，不要被業務拋的太遠



複雜度來源：高可用(High Availability)
==================================

- 高可用定義：**系統無中斷地執行其功能的能力，代表系統的可用性程度**，是進行系統設計時的準則之一

- 這個定義的關鍵在於"無中斷"，但恰好困難點也在"無中斷"上面，因為無論是單個硬體還是單個軟體，都不可能做到無中斷，硬體會出故障，軟體會有 bug；硬體會逐漸老化，軟體會越來越複雜和龐大 .... 等等

- 系統的高可用方案五花八門，但萬變不離其宗，本質上都是通過"**冗餘**"來實現高可用


## 計算高可用

計算有一個特點就是無論在哪台機器上進行計算，同樣的算法和輸入數據，產出的結果都是一樣的


## 存儲高可用

- 將數據從一台機器搬到到另一台機器，需要經過線路進行傳輸

- 除了物理上的傳輸速度限制，傳輸線路本身也存在可用性問題，傳輸線路可能中斷、可能擁塞、可能異常（錯包、丟包）

- 存儲高可用的困難點不在於如何備份數據，而在於**如何減少或者規避數據不一致對業務造成的影響**

- 存儲高可用不可能同時滿足 **[CAP("一致性、可用性、分區容錯性")](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86)**，最多滿足其中兩個，這就要求我們在做架構設計時結合業務進行取捨
  - Consistency
  - Availability
  - Partition tolerance


## 高可用狀態決策

- **系統需要能夠判斷當前的狀態是正常還是異常**，如果出現了異常就要採取行動來保證高可用

- 三種常見的決策方式：
  - **獨裁式**：當決策者本身故障時，整個系統就無法實現準確的狀態決策
  - **協商式**：協商式決策的架構不複雜，規則也不複雜，其難點在於，如果兩者的信息交換出現問題（比如主備連接中斷），此時狀態決策應該怎麼做
  - **民主式**：民主式決策還有一個固有的缺陷：腦裂(split brain)。

- 無論採取什麼樣的方案，狀態決策都不可能做到任何場景下都沒有問題，但完全不做高可用方案又會產生更大的問題，如何選取適合系統的高可用方案，也是一個複雜的分析、判斷和選擇的過程


## 討論整理精華

- 高可用與高性能，是架構設計中兩個非常重要的決策因素；因此在面對不同業務系統的不同需求，對高可用與高性能也會有不同的決策結論，其實現的複雜度也各不相同

- 支付寶業務，對於可用性和性能就會有很高的要求，在可用性方面希望能提供 7x24 不間斷服務，在高性能方面則希望能即時收付款；而對於一個學生管理系統，在可用性與性能方面就不一定要有多高的要求，比如晚上可關機，幾秒內能查詢到信息也可接受
> 高可用性與高性能的複雜度討論需要結合業務需求

- 可以利用百分比來對網站可用性進行度量：
  - 網站不可用時間 = 完成故障修復的時間點 - 故障發現的時間點
  - 網站年度可用時間 = 年度總時間 - 網站不可用時間
  - 網站年度可用性 = (網站年度可用時間/年度總時間) x 100%

> 高可用性就是技術實力的象徵，高可用性就是競爭力

- 為什麼會出現不可用 ?
  - 硬體故障：網站多運行在普通的商用 server ，而這些 server 本身就不具備高可用性，再加之網站系統背後有數量眾多 server ，那麼一定時間內 server 當機的機率自然會提昇，直接導致部署在該 server 上的服務受影響
  - 軟體 bug 或網站更新升級發佈：bug 不能消滅，只能減少；上線後的系統在運行過程中，難免會出現故障，而這些故障同樣直接導致某些網站服務不可用；此外，網站更新升級發佈也可能會引起相對較頻繁的 server 當機
  - 不可抗拒力：如地震、水災、戰爭...等

- 如何做到高可用? **核心思想：網站高可用的主要技術手段是服務與數據的冗餘備份與失效轉移**
  - 同一服務組件部署在多台 server 上
  - 數據存儲在多台 server 上互相備份

> 通過上述技術手段，當任何一台 server 當機或出現各種不可預期的問題時，就將相應的服務切換到其他可用的 server 上，不影響系統的整體可用性，也不會導致數據丟失

- 從架構角度看可用性：當前網站系統多採用經典的分層模型，從上到下為：**應用層**、**服務層**與**數據層**
  - 應用層主要實現業務邏輯處理
  - 服務層提供可復用的服務
  - 數據層負責數據讀寫

> 在部署架構上常採用應用和數據分離部署，應用會部署到不同 server 上，這些 server 被稱為應用層的 server；這些可復用的服務也會各自部署在不同 server 上，稱為服務層的 server；而各類數據庫系統、文件櫃等數據則部署在數據層的 server

- 硬體故障方面引起不可用的技術解決措施：
  1. 應用 server：可通過負載均衡設備將多個應用 server 構建為 cluster 對外提供服務(前提是這些服務需要設計為 stateless，即應用 server 不保存業務的上下文信息，而僅根據每次請求提交的數據進行業務邏輯的操作回應)，當 load balancer 通過心跳檢測手段檢測到應用 server 不可用時，則將其從集群中移除，並將請求切換到其他可用的應用服務上
  2. 服務層 server：這些 server 被應用層通過分佈式服務框架(例如：Dubbo)訪問，分佈式服務框架可在應用層客戶端程序中實現軟體負載均衡，並通過服務註冊中心提供服務的 server 進行心跳檢測，當發現有 server 不可用時，立即通知客戶端程序修改服務列表，同時移除回應的 server
  3. 數據 server：需要在數據寫入時進行數據同步複製，將數據寫入多台 server 上，實現數據冗餘備份；當數據 server 當機時，應用程序將訪問切換到有備份數據的 server 上

- 軟體方面引起不可用的技術解決措施：通過軟體開發過程進行質量保證
> 通過預發佈驗證、嚴格測試、Gray release 等手段，儘量減少上線服務的故障



複雜度來源：可擴展性(Scalability)
==============================

- 可擴展性(Scalability)指系統為了應對將來需求變化而提供的一種擴展能力，當有新的需求出現時，系統不需要或者僅需要少量修改就可以支持，無須整個系統重構或者重建

- 設計具備良好可擴展性的系統，有兩個基本條件：
  1. **正確預測變化**
  2. **完美封裝變化**

- 軟體系統與硬體或者建築相比，有一個很大的差異：**軟體系統在發佈後還可以不斷地修改和演進，這就表示不斷有新的需求需要實現**

- 預測變化的複雜性在於：
  - 不能每個設計點都考慮可擴展性
  - 不能完全不考慮可擴展性
  - 所有的預測都存在出錯的可能性

- 預測變化是一回事，採取什麼方案來應對變化，又是另外一個複雜的事情；即使預測很準確，如果方案不合適，則系統擴展一樣很麻煩

## 討論整理精華

- 什麼是架構的可擴展性? (What)
> 業務需求、運行環境方面的變化都會導致軟體系統發生變化，而這種軟體系統對上述變化的適應能力就是可擴展性

- 一個具備良好可擴展性的架構設計應當符合開閉原則：**對擴展開放，對修改關閉**

- 衡量一個軟體系統具備良好可擴展性主要表現但不限於：
  1. 軟體自身內部方面。在軟體系統實現新增的業務功能時，對現有系統功能影響較少，即不需要對現有功能作任何改動或者很少改動
  2. 軟體外部方面。軟體系統本身與其他存在協同關係的外部系統之間存在鬆耦合關係，軟體系統的變化對其他軟體系統無影響，其他軟體系統和功能不需要進行改動

- 為什麼要求架構具備良好的可擴展性? (Why)
> 伴隨業務的發展、創新，運行環境的變化，對技術也就提出了更多、更高的要求。**能夠快速響應上述變化，並最大程度降低對現有系統的影響**，是設計可擴展性好的架構的主要目的

- 如何設計可擴展性好的架構? (How)
> object oriented、design pattern 都是為了解決可擴展性的而出現的方法與技術

- 設計具備良好可擴展性的系統，有兩個思考角度：
  1. 從業務維度。對業務深入理解，對可預計的業務變化進行預測
  2. 從技術維度。利用擴展性好的技術，實現對變化的封裝

- 在業務維度。對業務深入理解，對業務的發展方向進行預判，也就是不能完全不考慮可擴展性；但是，變化無處不在，在業務看得遠一點的同時，需要注意:警惕過度設計；不能每個設計點都考慮可擴展性；所有的預測都存在不正確的可能性。

- 在技術維度。預測變化是一回事，採取什麼方案來應對變化，又是另外一個複雜的事情。即使預測很準確，如果方案不合適，則系統擴展一樣很麻煩
  1. 第一種應對變化的常見方案是將"變化"封裝在一個"變化層"，將不變的部分封裝在一個獨立的"穩定層"
  2. 第二種常見的應對變化的方案是提煉出一個"抽象層"和一個"實現層"

- 在實際軟體系統架構設計中，常通過以下技術手段實現良好的可擴展性：
  1. 使用分佈式服務(框架)構建可復用的業務平台
  2. 使用分佈式消息隊列降低業務模組間的耦合性

- 利用分佈式服務框架(例如：Dubbo)可以將業務邏輯實現和可復用組件服務分離開，通過接口降低子系統或模組間的耦合性
  - 新增功能時，可以通過調用可復用的組件實現自身的業務邏輯，而對現有系統沒有任何影響
  - 可復用組件升級變更的時候，可以提供多版本服務對應用實現透明升級，對現有應用不會造成影響

- 基於**生產者-消費者**開發模式，利用分佈式消息隊列(例如：RabbitMQ、Kafka)：
  - 將用戶請求、業務請求作為消息發佈者將事件構造成消息發佈到消息隊列
  - 消息的訂閱者作為消費者從消息隊列中獲取消息進行處理

> 通過這種方式將消息生產和消息處理分離開來，可以透明地增加新的消息生產者任務或者新的消息消費者任務。



複雜度來源：低成本、安全、規模
=========================

## 低成本

- 當我們設計"高性能" or "高可用"的架構時，通用的手段都是增加更多 server 來滿足"高性能"和"高可用"的要求；而低成本正好與此相反，我們需要減少 server 的數量才能達成低成本的目標

- 低成本很多時候不會是架構設計的首要目標，而是**架構設計的附加約束**

- 低成本給架構設計帶來的主要複雜度體現在 => **往往只有"創新"才能達到低成本目標**

- 引入新技術的主要複雜度在於**需要去熟悉新技術，並且將新技術與已有技術結合起來**

- 創造新技術的主要複雜度在於需要自己去創造全新的理念和技術，並且新技術跟舊技術相比，需要有質的飛躍

## 安全

從技術的角度來講，安全可以分為兩類：(`功能安全` & `架構安全`)

### 功能安全

- 常見的 XSS 攻擊、CSRF 攻擊、SQL 注入、Windows 漏洞、密碼破解等，本質上是因為系統實現有漏洞，hacker 有了可乘之機，功能安全其實就是"防小偷"

- 從實現的角度來看，功能安全更多地是和具體的編碼相關，與架構關係不大

- 功能安全其實也是一個"攻"與"防"的矛盾，只能在這種攻防大戰中逐步完善，不可能在系統架構設計的時候一勞永逸地解決

### 架構安全

- 架構安全就是"防強盜"

- 傳統的架構安全主要依靠防火牆，防火牆最基本的功能就是隔離網路，通過將網路劃分成不同的區域，制定出不同區域之間的訪問控制策略來控制不同信任程度區域間傳送的數據流

- 防火牆的功能雖然強大，**但性能一般**，所以在傳統的銀行和企業應用領域應用較多。但在互聯網領域，防火牆的應用場景並不多

- DDoS 攻擊最大的影響是大量消耗機房的出口總頻寬

- 不管防火牆處理能力有多強，當出口頻寬被耗盡時，整個業務在用戶看來就是不可用的

- 互聯網系統的架構安全目前並沒有太好的設計手段來實現，更多地是依靠運營商或者雲服務商強大的頻寬和流量清洗的能力

## 規模

- 規模帶來複雜度的主要原因就是"量變引起質變"，當數量超過一定的閾值後，複雜度會發生質的變化

- 常見的規模帶來的複雜度有：
  - 功能越來越多，導致系統複雜度指數級上升
  - 數據越來越多，系統複雜度發生質變

> 數據太多以後，傳統的數據收集、加工、存儲、分析的手段和工具已經無法適應，必須應用新的技術才能解決


## 討論整理精華

- 低成本是架構設計中需要考慮一個約束條件，但不會是首要目標。低成本本質上是與高性能和高可用衝突的，當無法設計出滿足成本要求的方案，就只能協調並調整成本目標

- 一般通過"創新"達到低成本的目標
  - 引入新技術。主要複雜度在於需要去熟悉新技術，並且將新技術與已有技術結合；一般中小型公司基本採用該方式達到目標
  - 開創一個全新技術領域。主要複雜度在於需要去創造全新的理念和技術，並且與舊技術相比，需要有質的飛躍，複雜度更高；一般大公司擁有更多的資源、技術實力會採用該方式來達到低成本的目標

- 安全是一個龐大而又複雜的技術領域，一旦出問題，對業務和企業形象影響非常大。從技術的角度來講，包括：
  - 功能安全("防小偷")，減少系統潛在的缺陷，阻止 hacker 破壞行為
  - 架構安全("防強盜")，保護系統不受惡意訪問和攻擊，保護系統的重要數據不被竊取。由於是蓄意破壞系統，因此對影響也大得多。架構設計時需要特別關注架構安全。

- 規模帶來複雜度的主要原因就是"量變引起質變"，當數量超過一定的閾值後，複雜度會發生質的變化
> 隨著業務的發展，規模帶來的常見複雜度有：(1)業務功能越來越多，調用邏輯越來越複雜；(2)數據容量、類型、關聯關係越來越多

- 規模問題需要與高性能、高可用、高擴展、高伸縮性統一考慮。常採用"分而治之，各個擊破"的方法策略

- 是否還有其他複雜度原因？ => **可伸縮性**

- 當前大型互聯網網站需要面對大量用戶高並發訪問、存儲更多數據、處理更高頻次的用戶交互。網站系統一般通過多種分佈式技術將多台 server 組成集群對外提供服務。伸縮性一般是系統可以根據需求和成本調整自身處理能力的一種能力

- 伸縮性常意味著系統可以通過低成本並能夠快速改變自身的處理能力以滿足更多用戶訪問、處理更多數據而不會對用戶體驗造成任何影響

- 伸縮性度量指標包括：
  - 處理更高並發
  - 處理更多數據
  - 處理更高頻次的用戶交互

- 伸縮性複雜度體現在：
  - 伸：增強系統在上述三個方面的處理能力
  - 縮：縮減系統處理能力
  - 上述伸縮過程還必須相對低成本和快速


Reference
=========

- [從0開始學架構_架構基礎_架構入門-極客時間](https://time.geekbang.org/column/intro/81)