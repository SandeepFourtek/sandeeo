###

|
###

######
# Design Specification

  **Version 1.0a**   |
| --- |




#### Taobao Ring System

###### **A Taobao Agent Application**





**Sponsor:**

Fourtek IT Solutions pvt. Ltd.

**Development Team:**

Vipin Kumar

Himanshu Sharma

Sandeep Soni

Neha Sharma

Arpit Sharma





###### 07:42:48



###### Table of Contents

**Introduction………………………………………………………………………………       **

**System Overview…………………………………………………………………………****        **

**Design Considerations…………………………………………………………………...       **

**Assumptions and Dependencies………………………………………………………****....       **

**Goals and Guidelines…………………………………………………………………….       **

**Architectural Strategies………………………………………………………………….       **

**System Architecture……………………………………………………………………...       **

_Data parsing ………………………………………………………………………………_

_TRGUI (Taobao Ring GUI) Interface……………………………………………………_

_TRDB (Taobao Ring Database)…………………………………………………………..       _

**Dependencies………………………………………………………………………………       **

_3_
_# rd_ _ Party dependencies ………………………………………………………………….....    _

_Api Dependencies ………………………………………………………………………....._

_Plugins……………………………………………………………………………………..._

_Language……………………………………………………………………………………_

_Server Enhancements…………………………………………………………………….._

**Detailed System Design………………………………………………………………….       **

_ **Major functions** _

_Data parsing……………………………………………………………………………….               _

_Order state transition diagram……………………………………………………………._

_Ticketing system,………………………………………………………………………….       _

_Coin system…………………………………………………………………………………_

_Notification system………………………………………………………………………..._

_Email system………………………………………………………………………………       _

_Email templates……………………………………………………………………………               _

_Omni editor…………………………………………………………………………………       _

_Translate……………………………………………………………………………………_

_Photo upload……………………………………………………………………………………_



_ **User Interface section** _

_Shopping cart………………………………………………………………………………._

_Payment forms………………………………………………………………………………_

_Order lists ………………………………………………………………………………….._

_History Order List ………………………………………………………………………….._

_FAQ…………………………………………………………………………………………._

_News pages.. …………………………………………………………………………………_

_Feedbackpages.. …………………………………………………………………………….._

_Search .. ……………………………………………………………………………………..._

_Blog.. …………………………………………………………………………………………_

**Administrator pages**

_Admin order page.. ………………………………………………………………………….._

_Admin user page.. ……………………………………………………………………………_

_Admin feedback page .. ………………………………………………………………………_

_Admin FAQ.. …………………………………………………………………………………._

_News pages.. ………………………………………………………………………………….._

_Ticketing system.. ……………………………………………………………………………._

_Admin news page.. ……………………………………………………………………………_

_Admin employement page.. ………………………………………………………………….._

_Admin search.. ……………………………………………………………………………….._

_Admin blog.. …………………………………………………………………………………_

**Glossary………………………………………………………………………………………       **

**Acronyms and Abbreviations…………………………………………………………. …...**

**Bibliography………………………………………………………………………………….**



### [Introduction](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC4)

_This document is designed to be a reference for any person wishing to implement or any person interested in the architecture of the Taobao Ring client application,  and  the Taobao Ring database.  This document describes each application&#39;s architecture and sub-architecture their associated interfaces, database schemas, and the motivations behind the chosen design.  Both high-level and low-level designs are included in this document._

_This document should be read by an individual with a technical background and has experience reading data flow diagrams (DFDs), control flow diagrams (CFDs), interface designs, and development experience in object oriented programming and event driven programming._

_This design document has an accompanying specification document and test document.  This design document is per Taobao Ring PRD version 2.  Any previous or later revisions of the specifications require a different revision of this design document._

_This document includes but is not limited to the following information for the Taobao Ring System; system overview, design considerations, architectural strategies, system architecture, Dependencies, and detailed system design._



### [System Overview](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC5) diagram

TO BE DESIGNED





The Taobao Ring System



### [Design Considerations](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC6)

_This section describes many of the issues that needed to be addressed or resolved before attempting to devise a complete design solution._

#### [Assumptions and Dependencies](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC7)

_This design of the Taobao Ring system makes several assumptions about software has several software dependencies.  All environmental requirements of both the server and client applications can be found in the Taobao Ring System Requirements 3.1._

_The server applications make the following assumptions about their environmental environments;_

- _The system can be described by the environmental requirements associated to this document._
- _The system the application is executing on will have the required resources available as necessary.  This entails sufficient memory and permanent storage space, an adequate CPU for the necessary application, and a TCP/IP network connection._

_The Server application makes the following assumptions about its operation environment.       _

- _The server machine will have Cent OS 7,php.5.6  , MySql  and Apache configuration._

- _ The application is dependent on this set of component.  These components are required for our implementation of access to the dialog database._
- _The client machine will have the necessary databases setup through ODBC (Open DataBase Connectivity)._
- _The server machine will have its own mail server or any use third party mail server, If preffered we can use Server own mail for communication. Otherwise we can use mailgun or gmail._
- _The server machine will have many php package (eg geoip)._
- _Preferably the server machine will have TCP port 12345 free for use of the server application.  This is the default port for the server to listen on, though it is not required to listen on this port._

#### [Goals and Guidelines](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC9)

_The major goal of the Taobao Ring client is that it be extremely simple and intuitive to use.  The application is geared towards the Shopping enthusiast, not a technically inclined individual.  It is very important that the prompts for the user be clear and concise since this will be the highest level of interaction between the application and the user.  It is also important that series of prompts and responses be tested with users before being deployed as part of the product so that all interaction is &quot;approved&quot; by a potential user._

_ _

_The second major goal of the application is that the user gets a response in a timely fashion.  Intuition tells that a user will lose interest if they have to wait long times for software to respond.  This is why the design has minimal data transferred between client and server.  In this design, a minimum set of information is transferred to the server in order to retrieve the necessary information, and the server only returns the requested data that is then formatted into a readable phrase on the client side._

_A third major goal is that the client application could possibly be smooth functioning of the emailing systemand inbuild ticketing system with emails. We have to take care of the GFW Problems._

_The fourth major goal is that application will reduced to only 2 mirror servers and flow of the information should be smooth enough._

_A fifth major goal is that application would be able to split the orders and can do part shipping._

_This design attempted to keep the client application as data independent as possible.  All prompts and responses on the client side are completely data driven, so any prompt or response can be changed by a simple admin command change.  This makes the client capable of prompting and responding to any structural type of data.  _

_The Taobao Ring server is intended to have a simple interface that is relatively easy to administer.  A minimal yet complete set of options is provided for the server administrator to have control of resources consumed by the server application.  These options include, but are not limited to; controlling the limit of clients able to connect to the server for maximum efficiency, ability to configure which port the server listens on, ability to change the Taobao Ring database location, and control how often the database is updated._

### [Architectural Strategies](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC11)

_The Taobao Ring system design has been divided into Two major sub-systems; server application, Taobao Ring database.  The server application is then separated into Three major sub-sections; The parsing , server communications, server GUI (Graphical User Interface)._

_The server application&#39;s major design considerations include easy Taobao Ring data retrieval, easy database updates, multiple users support, and a minimal set of administrative features. The server application has been designed to be as flexible as possible, trying not to design the server for specifically Taobao Ring information, but for any type of information.  Given the project&#39;s constraints of human resources, software resources, and time, the server is not completely &quot;data independent&quot;.  Portions of the server application are specific to this Taobao Ring system.  These portions are discussed in the server application&#39;s detailed design strategies._

### [System Architecture](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC12)

#### [Subsystem Architecture](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC13)



1. 1)Data parsing



**Retrieve HTML files** :

_We&#39;ll first retrieve data_ _(actively or passively? Actviely means we have a crawler to get all the pages from these site; Passively means we crawl page only when customer input target URL. If passively, the input should be URL, not HTML files.)_ _from taobao.com, tmall.com, jd.com, 1688.com, dangdang.com and amazon.cn_ _(if the data retrieved OK, then use this data as the input of the parse subroutine.) Here we will retrieve all data passively. If the data retrieved OK the use this data as the input of the parse subroutine.__Otherwise, log errors._

**Parse HTML files** :

_We&#39;ll go though each tag of the HTML file, check if the tag contains useful data; if so parse the tag and get the data._

**Format outputs** :

_Put parsed data in the format that we&#39;ll discuss in the interface section__( list all the data given below)__. Then check if we get the correct data. The reason that we wait until this part to check the data instead of doing that right after w __e get the data is efficiency. We don&#39;t want spending too much time checking data. If the data is correct, then write it to_ _ database Otherwise, we&#39;ll parse the file again to get data__. Otherwise, log errors._

**On Format Output we will get response of these fields , those are shown below**

_ _ **`** crawler\_time **`** ,

  **`** item\_url **`** ,

  **`** item\_options **`** ,

  **`** weight **`** ,

  **`** image\_url **`** ,

  **`** name **`** ,

  **`** description **`** ,

  **`** description\_english **`** ,

  **`** message **`** ,

  **`** tbrmessage **`** ,

  **`** seller **`** ,

  &#39;quantity&#39; ,

  **`** price **`** ,

**File maintenance**

_     Create the directories_ **data** _and_ **logerr** _under the directory contains the programs to store results and log errors, respectively._



**2 - TRDB (Taobao Ring Database)**

_The Database is implemented using MySql.  It functions as storage to keep track of the entire users credentials, order associated with each user and the schedule of date and time of orders shipping and tickets mangament._

_The Database is implemented using MySql. Set of commands we&#39;ll be using for the rest of  tell MySQL what to do is part of a standard called Structured Query Language, or SQL . MySQL is the database server software that we are using.  _

_ Database Server Layer_

_Concurrency: Some applications have more granular lock requirements (such as row-level locks) than others. Choosing the right locking strategy can reduce overhead and therefore improve overall performance. This area also includes support for capabilities such as multi-version concurrency control or &quot;snapshot&quot; read._

_Transaction Support: Our application needs transactions, there are very well defined requirements such as ACID compliance and   more._

_Referential Integrity: The need to have the server enforce relational database referential integrity through DDL defined foreign keys._

_Physical Storage: This involves everything from the overall page size for tables and indexes as well as the format used for storing data to physical disk._

_Index Support: Different application scenarios tend to benefit from different index strategies. Each storage engine generally has its own indexing methods, although some (such as B-tree indexes) are common to nearly all engines._

_Memory Caches: Our applications respond better to some memory caching strategies._

_So although some memory caches are common to all storage engines (such as those used for user connections or MySQL&#39;s high-speed Query Cache)._

_Performance Aids: This includes multiple I/O threads for parallel operations, thread concurrency, database checkpointing, bulk insert handling, and more._

_Miscellaneous Target Features: This may include support for geospatial operations, security restrictions for certain data manipulation operations, and other similar features._

_Database schema will be attached soon._



**3 – GUI  (Graphical user Interface)**

_The GUI (Graphical User Interface) is how the Users and  administrator interacts with the Taobao Ring website.  The GUI provides the details/statistics about the User , orders, shipment, products  and number of clients currently connected etc..  _

_The administrator is provided the ability to to check all orders , shippment , tickets etc._





### [Dependencies](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC14)

** Third party dependencies**

1. 1)_Email provider – We  need to select the correct email provider because of  GFW , we have many contenders in it, gmail , mailgun and server own email system.  We will review these services with help of client to choose the correct provider .       _
2. 2)_Language Translator – We need to use translator for translating chinese to english. Current site use bing convertor, we will use the same for the new website as well._
3. 3)_Main dependent PHP libraries – Geoip , Maxmind_
4. 4)_Crawler library_
5. 5)_Page parse libarary_
6. 6)_Image processing library_
7. 7)_Barcode generate and decode libarary_



**API dependencies  **



1. a)_Basic API  ---_

_        login API via social media (FB, twitter, G+)_

_        Payment apis_

1. b)_Advance API_

_         Discussion with client remaining._

_Ex : taobao API , 17 track API , 4px API_

**Plugins**

**       ** _Livechat, GA,_

**Server Enhancement**

1. a)_Use  of CDN such as clould flare_
2. b)_Use of memcache_
3. c)_Use of_ _Php-FPM_
4. d)_Use Zend Opache_
5. e)_Use Apcu_



### [_Detailed System Design_](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC15)

_1) Data Parsing :_

_Classification_

_Modular subsystem of the application._

_Purpose_

_This class implements the html parser/stripper necessary to derive Shopping information from the associate websites._

_Uses/Interactions_

_This module is to be used by website users and the website admin._

_Method_

_Data parsing_

###### Purpose

_To fetch the products from other sites to our sites and translate them to english._

###### Parameters

_         To be shared._

###### Assumption: User knows website process and how our website works.

###### Explanation:

Data parsing Front end :

###### Our website will have the ability to crawl the below websites mentioned below
taobao.com, tmal.com,1688.com, jd.com etc..

######


![untitled](https://cloud.githubusercontent.com/assets/16163749/19684073/dd3b50e6-9ad2-11e6-8a0c-f4dbd04e7b15.png)


_Explanation_

_P1 User will enter URL_

_P2 System will check whether the URL is correct or not. If URL is not correct then stop and didn&#39;t show anything._

_If URL is correct then go to P-3_

_P-3 Now we need to start crawling_

_P-4 Is syntax of DOM element is correct then got P-5 and show JSON response._

_    If syntax is not correct then we need check backend problem._

** what backend problem:-** _Sometime we will see that particular website has changed is DOM pattern that time we need to update structure of DOM element via Admin._

_P-5 Execute using DOM library and generate response_

_P-6 Get JSON in response_

_P-7 Append response with DOM element and show in front end HTML._ _We should have retry strategies, because sometimes we&#39;ll get empty page or incorrect page for the cookie or other reasons, we have to crawling again_

**Crawling Process from Admin**

![untitled](https://cloud.githubusercontent.com/assets/16163749/19685176/c9fd2cc4-9ad8-11e6-953c-14f35459ecd4.png)


_Explanation_

_P-1 Go to Crawling tab in admin._

_P-2 Select option:- for which website you want to change crawling pattern. Because all website does have different-different DOM structure.
P-3 After that you need to know the structure of DOM that we are using in our algorithm. Then we need to fill that same structure. We_ _should support Regular Express to described DOM structure__.
P-4 when all things are done, then we can test our pattern filled by us in P-3. For this we need and product page URL of that particular website._

_P-5 If URL is working right then._

_P-6 it show an JSON with given Structure._

_P-7 If JSON is not correct it means we need to check our DOM structure that we have entered in P-3_

_P-8 If JSON is correct then we can update and can check at front-end_





2) _Order state transition :_



Classification

Modular subsystem of the application.

Purpose

This class tell us necessary Shopping information flow and the order details by the cutomer. It is one of the biggest function of our application.

Uses/Interactions

This module is to be used by website users and the website admin.

Method

To be modified

###### Purpose

Making order smooth.









Diagram order state transition

####

![untitled](https://cloud.githubusercontent.com/assets/16163749/19685228/28d18da8-9ad9-11e6-8cf7-080465344477.png)

###### Explanation:  Once the admin receives the order (order),Admin will recevie the email of the order we can switch off this email notification; otherwise we&#39;ll receive hundreds of emails, Admin will assign a admin agent for this order,All employees can assign orders to any one else. If order is in stock , than the user will go for the 1 payment directly.

_S0, wait\_user\_confirm: _

_User can add items into their shopping cart, and save the order for later use._

_NEXT STEP:_

_a. Submit order, then status changes to wait\_admin\_confirm._

_b. If items are come from our warehouse, status changes to  wait\_user\_pay1._

_S1, wait\_admin\_confirm:_

_Once user think it&#39;s the time to submit the order, then after the submit action, order status will changes to wait\_admin\_confirm._

_NEXT STEP:_

_a. Users can change orders, address, etc. then back to wait\_use\_confirm._

_b. We can confirm the order and send invoice to users, then status changes to wait\_user\_pay1._

_S2, wait\_user\_pay1_

_We will confirm items with sellers about color, size, price, domestic delivery fee, etc. Once all items are confirmed, we&#39;ll send users the invoice and order status changes from wait\_admin\_confirm to wait\_user\_pay1._

_NEXT STEP:_

_a. Users can change the shopping cart, items numbers, or remarks, and order status will change back to wait\_user\_confirm._

_b. Users can make payment and going to user\_pay1._

_S3, usr\_pay1_

_Users can make payment via PayPal, Western Union, MoneyGram, WebMoney, Bank transfer, WorldRemit, etc. _

_NEXT STEP:_

_a. If the payment status can be confirmed automatically, just like PayPal IPN, order status will changes to next step: purchasing. _

_b. If the payment cannot be confirmed instantly, such as PayPal eCheck, Western Union, etc., the order status will stay in user\_pay1._

_c. If the payment failed, status will changes to wait\_user\_pay1. _

_S4, purchasing_

_PayPal or other online payment can make order status changes from user\_pay1 to purchasing. But others should waiting for our confirmation with payment and updated manually. We&#39;ll buy all purchasing status orders within 12 hours._

_NEXT STEP:_

_a. After our purchase, the sellers will send us items. We&#39;ll check them one by one until all items arrived, the status will be all\_items\_arrived._

_b. If all items are out of stock, order status will changes to end._

_c. If it&#39;s virtual items, order status will change to shipped. _

_d. If the items will be delivered to users directly, order status will change to shipped._

_S5, all\_items\_arrived_

_When all items arrived, order status will be changed to this status. We&#39;ll take photos, pack items, and get weight and volume size._

_S6, wait\_user\_pay2_

_We&#39;ll send users then 2nd payment invoice, it&#39;s mainly about international shipping fee, and some refund or the fees users should pay more cause of items added after 1st payment._

_NETX STEP:_

_a. The normal process is user make payment and changes from wait\_user\_pay2 to user\_pay2._

_b. If the refund amount is more than 2nd payment invoice, users can click &quot;Ship it&quot; and change status from wait\_user\_pay2 to wait\_ship directly._

_c. If the 1st payment include 2nd payment, users can click &quot;Ship it&quot; and it&#39;s the same as (b)._

_S7, user\_pay2_

_Just like user\_pay1. Status can change to wait\_ship automatically, or waiting for update manually. _

_S8, wait\_ship_

_We&#39;ll ship parcels within 12 hours._

_S9, shipped_

_After ship the parcel, we&#39;ll upload the shipping form scan file, update tracking number and our shipping fee to the system._

_S10, received_

_User can change order status to received, or if the auto checker thread find it&#39;s received by users, will change to received automatically._

_S11. end_

_We can set order status to end._

_S12. user\_direct\_buy_

**This process will be refined again.**



3) Ticketing System

 _Classification_

Modular subsystem of the application.

_Purpose_

This class implements the ticket system, user can raise tickets to the admin users after the  Shopping in the website. They can give their problemsor suggestion through system.

_Uses/Interactions_

This module is to be used by website users and the website admin.

_Method_

Ticket raise system

###### Purpose

To make easy flow of information between client and support staff.

###### Parameters

######          To be decided

###### Assumption:

######  User know how to raise tickets.

###### Explanation:  Data flow designed below


![untitled](https://cloud.githubusercontent.com/assets/16163749/19685274/61be9336-9ad9-11e6-8908-eadccf8a34ee.png)


_Explanation_

_P-1  Check login user :-- User is signIn our website that time he can raise a ticket. If user is not login then he/she can&#39;t raise the ticket  _

_P-2. After login Customer can raise ticket_

_P-3 Fill all related information of order, that customer want to ask  to administrator__.Ticket can associated with order, item, tracking number or nothing; And they can fire tickets from order list, item list or other &quot;Support&quot; buttons in our fronted pages._

_P-4 Submit that form , After instant time customer will receive an auto-generated email with ticket-id._

_P-5 After generated ticket number, Admin will assign that ticket to a particular agent_ _.Website can assign ticket to agents automatically, depends on the order status and who confirmed the order, etc. We can also reassign to another agent manually__._

_P-6 Agent will work on that ticket._

_P-7 After that agent will check, is this ticket resolved ? . if ticket is resolved then go to p-8, Otherwise it will go to p6 again._

_P-8 Need to close the ticket_

_More requirements about tickets:_

1. _Support omni-editor._
2. _Support search._
3. _Support pending(open)/assigned/waiting customer reply/closed/deleted status._
4. _Auto reply (like tickets in Holiday)_
5. _Auto assign_
6. _Reassign and private notes._
7. _Convert emails to tickets._
8. _Support flag to mark tickets. (Because we have lots of tickets)_

_You can refer our current 3rd part Ticket system._ _We only need a simpler implement__, but easy to use._





![untitled](https://cloud.githubusercontent.com/assets/16163749/19685350/afced86a-9ad9-11e6-8318-431312527559.png)


### [_CONDITION AND ACTION_]

![condition and action](https://cloud.githubusercontent.com/assets/16163749/19688274/b1fdb7d4-9ae6-11e6-9b60-2fb09b851156.png)


_1) If email contains same subject then that we given then admin can assign two things those are.
Assigned automatically to agent.
Deleted automatically


    
For eg: If the email has subject  ‘invoice ‘ then the ticket is assigned to agent those are handling invoice .


_2) If email condition has same email ID on which we want to perform any action then we use this condition and action


For eg: if email ID is xxxxx@xxx.xx then the ticket is assigned to admin.


_3) Requester email- If contains subject as xxxx then the ticket can be assigned to the agent or can perform delete operation
If requester email contain any email then the ticket can be assigned to the agent or can perform delete operation

### [_TICKET STATUS_]

Section a)

![untitled](https://cloud.githubusercontent.com/assets/16163749/19688828/db02235c-9ae8-11e6-8c0b-fbb930ea75ed.png)


Section b)


![untitled](https://cloud.githubusercontent.com/assets/16163749/19688874/09eccda2-9ae9-11e6-82ba-480f7999c85f.png)

            After auto-generated ticket.
When ticket comes first time in admin it’s status is unassigned and ticket will represent as NEW
If ticket is not assigned to any agent after fixed time then the status of the ticket  is ‘pending’
If ticket is assigned automatically to agent or assigned by admin then status of ticket is Assigned
If ticket is solved then the status is ‘solved’
If ticket is not solved and take time then agent can change status of  ticket to ‘hold’.
If ticket is successfully solved then agent can change status of  ticket to closed
If the query seems to be still unsolved and ticket status is closed then we can give ticket status as  ‘Reopen’ and Assign to agent after that same process will followed



### [_HOW TICKET SYSTEM WORKS_]

![untitled](https://cloud.githubusercontent.com/assets/16163749/19688951/57642e68-9ae9-11e6-84c9-ffa994fdd28a.png)

If user’s login is successful then go to my account, then from the order list user can see the list of all orders and then raise ticket.

If user’s login isn’t successful then process is stopped.

In the ticket user will fill all these information  which includes subset/summary,select order,select multiple item,file description.

If user submit all these information then a mail is generated automatically and the user receive a ticket Id and auto-generated mail.
 
### These are other functionality that use have:- 
get notification on user’s account
can see all ticket details in my account
User can reply on ticket using email or from my account
Using settings, user can off the notification.


### 4) Wallet coin System_

_ __Classification_

_Modular subsystem of the application._

_Purpose_

_This class implements the virtual money user can spend on  Shopping in the website._

_Uses/Interactions_

_This module is to be used by website users and the website admin._

_Method_

_Coin system_

###### Purpose

_To make payment without using actual payment gateway._

###### Parameters

######          To be decided

###### Assumption:

######  User know how wallet system has been used.

_Explanation:  Data flow designed below_



_Coin wallet system , It is designed in Three part ._

_1)Coin wallet system_

_2) Use wallet amount_

_3) Withdraw wallet money in user account._

**This diagram is not clear and seems logic mess. Coin payment just like PayPal, if customer need payment, they can use coin wallet as PayPal. Don&#39;t miss the order process with coin system.**

_Actually here we concentrate on only coin system .. We will make complete order flow if you required. Because you already has given us order diagram._

![untitled](https://cloud.githubusercontent.com/assets/16163749/19685499/996c6d2a-9ada-11e6-803e-207652557638.png)

_Explanation_ _(You missed coin with order process)_

_P1:User creates an order_

_P2 :- Admin will Check whether we received first payment or not . if admin not receive any payment then wait for first payment._

_P3 :- After that we will check item is received by admin or not_

_P3 :- All items are received then we will check for second payment. If second payment is not required then go to P-10 and start shipping process.
                If second payment required then got P9 and wait for second payment  _

_P9 :- wait for second payment_

_P10 :- if second payment received,  after that item to be process for shipped_

_P4:- if items are out of stock,  then admin should refund the payment. Payment can be refund by two methods_

- _Using coin system_
- _Using paypal_

_P5 :- If item prices are changed then we need to refund extra money. After comparing international shipping fee, Admin should refund the payment. Payment can be refund by two methods_

- _Using coin system_
- _Using paypal_

_P6:- if items are partially arrived then we need to refund extra money. Admin should refund the payment. Payment can be refund by two methods_

- _Using coin system_
- _Using paypal_



















**Use Wallet Amount**** (I don&#39;t understand. Do you means wallet account? Or wallet balance? Or make payment with wallet? Wallet amout(balance?) is very simple, we can go though the coin trades to get the wallet balance. I don&#39;t understand the below diagram.)**

Here Wallet is related to coin system. This digram include when user wants to pay order amount with coin. There are some scenario

- if customer is paying full amount with coin
- if customer is not paying full amount with coin
- if customer is paying full amount with coin and Coins are not equal to order price

1. if user is paying his order amount with coin:- If user don&#39;t use full amount that time we need to calculate actual price that customer wants to redeemed from is coins.
2. (update wallet means update coin system) If user is paying with coin and after that he is not satisfied with that amount. That time he can reuse his wallet

![untitled](https://cloud.githubusercontent.com/assets/16163749/19685548/db8c4b30-9ada-11e6-8f2b-6d75a0e313a4.png)

_Explanation_

_P-1 User will Create order_

_P-2 Check amount in wallet :- if amount is not available then customer need to pay with paypal_

_P2. if amount is available then customer can pay with paypal or wallet .
         For wallet go to P3
P3.  If customer is using full amount then wallet is automatically empty and customer will receive an email of wallet._

_P3. If customer not paying full amount then Redeem his wallet balance amount. It means customer will fill how much amount he/she wants to redeem from his/her wallet._

- _If price are successfully redeemed then compare with order price_ _(here is the payment only via coin)__._
- _If order price are greater than redeemed wallet price then customer need to pay extra money, For this customer can use two method_ _(here is the partial payment with coin)_
- _Wallet_

_                  If Customer has more amount in his wallet then he/she can pay with wallet_

- _PayPal_ _(and other offline payment methods)_
 _     Either customer is having amount in his wallet or not. That time customer can pay with paypal as well_

**Withdraw Wallet Money in his account**


![untitled](https://cloud.githubusercontent.com/assets/16163749/19685609/1b525fc0-9adb-11e6-87a0-16c8cbbfd721.png)

_P1 Go to wallet
P2 check wallet, if wallet having money then_

_P3 want to transfer his coin money to Paypal account_ _(We only support withdraw money to customer&#39;s PayPal account)__.
P4 Fill related Information and submit form_ _(PayPal account &amp; amount to withdraw)__._

_P5 After submit information admin should receive notification.
P6 Admin will check customer information_

_P7 Admin will check amount in customer&#39;s wallet. If amount is available then go to P-8_

_P8 transfer coin balance in his bank account. And redeem that amount from customer wallet._

_5) Notifications_

_Classification_

_Modular subsystem of the application._

_Purpose_

_This class implements the communication alert between user and admin._

_Uses/Interactions_

_This module is to be used by website users and the website admin._

_Method_

######              Notification

###### Parameters

######          To be decided

_Basic requirements:_

- _All notifications should along with emails_
- _Some notifications can be switched on or off. _

_Scenario:_

- _When order/payment status changes by us. Send notifications to customers._
- _When order/payment status changes by customers. Send notifications to us._
- _When add important comments to items. Send notifications to customers._
- _When customers update item&#39;s remark, or order message after the confirmation status (after wait\_admin\_confirm), send notifications to us._
- _When we send coupon to customer&#39;s account._
- _When customers take important actions, like reset email/password, etc._
- _When we publish important news, like holiday off-duty, or exchange rate, service rate change._
- _When customer&#39;s account changed, like from normal to VIP._
- _We can send message to customers. And the messages should along with notification._
- _We can send message to colleagues. And the messages should along with notification._

FLOW  DIAGRAM:

_Flow will be provided later._





### [Glossary](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC17)

_Query Strings_ _are the final result of the user&#39;s interaction with the application.  They are sent to the Taobao Ring Server where they are interpreted and acted upon._

_Prompts_ _will be defined as the point at which the computer and the user interact.  These are decision points in the control flow of the program, allowing the program to branch based on the user response._

_Prompt Text_ _is the text that is read by the computer to the user at any given prompt.  There may be several levels of prompt text for any prompt; the actual prompt text read to the user depends on the current user level._

_Commands_ _are the legal responses the user may make at any given prompt._

_Scripts_ _are series of prompts that are executed in succession.  They may be used when multiple pieces of information must be determined in order to form a query string._

_Help Menus_ _are sets of text that may be read to the user when help is requested at any prompt or at a global level._

_Help Text_ _is the text read by the computer to the user at any given help menu.  There may be several levels of help text for any help menu; the actual help text read to the user depends on the current user level._

_Global_ _is the term used to identify commands and help menus that are available at any prompt within the system._

_Local_ _is the term used to identify commands and help menus that are available only when the user is at a specific prompt._



**Acronyms and Abbreviations**

_     WV                        Data parse_

_     RBDS                Taobaorings database system_

_     DI                        Database Interface_

### [Bibliography](http://www.enteract.com/~bradapp/docs/sdd.html#TOC_SEC18)

Software Design Template, by Sandeep

Web Programming with Php, by Vipin