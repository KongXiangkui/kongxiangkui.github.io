---
title: Web基础
date: 2020-04-13 14:48:04
tags:
- Web
categories:
- [Web]
- [网络]
toc: true
---
本文记录Web的基础概念
<!-- more -->
## Internet
> A technical infrastructure which allows many computers to be connected all together.  

*Router*: has only one job, like a signaler at a railway station, it makes sure that a message sent from a given computer arrives at the right destination computer.

*modem*: turns the information from our network into information manageable by the telephone infrastructure and vice versatility.
> The next step is to send the messages from our network to the network we want to reach.  To do that we will connect our network to an Internet Service Provider (ISP).  

*IP(Internet Protocol) address*: IPV4 address (e.g. 173.194.121.43) or IPV6 address (e.g. 2021:0da8:8b73:0000:0000:8a2e:0370:1337)

*domain name*
- TLD, Top-Level Domain: tell users the general purpose of the service behind the domain name (.com, .org, .net, .edu, .us).
- Label (or component): follow the TLD. A label is a case-intensities character sequence anywhere from one to sixty-three characters in length, containing only the letters, digits and the - character(which may not be the first or last character in the label).
> The label located right before the TLD is also called as a Secondary Level Domain (SLD).  
> A domain name can have many labels (or components).  

*URL, Uniform Resource Locator*
> Absolute URLs vs relative URLs  
> Semantic URLs use would with inherit meaning that can be understood by anyone, regardless of their technical know-how.  
- The first part of the URL indicates which protocol the browser must use.
> A protocol is a set method for exchanging or transferring data around  a computer network. (web requires one of http: and https:, but browsers also know how to handle other protocols such as mail to: (open a mail client) or ftp: to handle file transfer.  
- Domain name
> www.example.com  
- Port
> :80 is the port, which is used to access the resources on the web server.  
> http和https中可以省略，http是80， https是443  
> 其他事强制的  
- Path to the resource on the web server.
> /path.to/my file.html  
- Parameters
> ?key1=value1&key2=value2  
- Anchor锚
> #SomewhereInThe Document  
> An anchor represents a sort of “bookmark” inside the resource, giving the browser the directions to show the content located at the “bookmarked” spot.  

> http://www.example.com:80/path/to/my file.html?key1=value1&key2=value2#SomewhereInThe Document  

### DNS
DNS database are stored on every DNS server worldwide, and all these servers refer to a few special servers call “authoritative name servers” or “top-level servers.”
Whenever your registrar creates or updates any information for a given domain, the information must be refreshed in every DNS database.
### Web Server
> Some computer (called web servers) can send messages intelligible to web browser.  
- Hardware: A computer that stores web server software and a website’s component files.
- Software:  control how web sets access hosted files, at minimum an HTTP server (A piece of software that understand URLs and HTTP)
![](Web/%E7%85%A7%E7%89%87%202020%E5%B9%B42%E6%9C%8819%E6%97%A5%20%E4%B8%8A%E5%8D%8892816.jpg)

- A static web server, or stack, consist of a computer(hardware) with an HTTP server(software). We call it “static” because the server sends its hosted files “as- is” to your browser.
![](Web/%E7%85%A7%E7%89%87%202020%E5%B9%B42%E6%9C%8819%E6%97%A5%20%E4%B8%8B%E5%8D%8814653.jpg)

- A dynamic web server consists of a static web server plus extra software, most commonly an application server and a database. We call it “dynamic” because the application server updates the hosted files before send them to your browser via the HTTP server.(fill an HTML template with contents from a database. A few HTML templates and a giant database——make it easier and quicker to maintain and deliver the content.)
![](Web/%E7%85%A7%E7%89%87%202020%E5%B9%B42%E6%9C%8819%E6%97%A5%20%E4%B8%8B%E5%8D%8814714.jpg)



### Search Engine
A website that helps people find web pages from other websites.

### HTTP Request:
*A URL* identifying the target server and resource
*A method* that defines the required action (for example, to get a file. Or to save or update some data).
- GET: get a special resource
- POST: create a new resource
- HEAD: get metadata information about a special resource without getting the body like GET would
> Use a *HEAD* request to find out the last time a resource was updated, and then only use the *GET* request to download the resource if it has changed.  
- PUT: update an exiting resource (or create a new one if it doesn’t exist).
- DELETE: delete the specified resource.
- TRACE,OPTION,CONNECT,PATCH
*Additional information* can be encoded data in the request
- URL parameters
> ?    separate the rest of the URL from the URL parameters  
> =   separate each name from its associated value   
> &.  separate each pair  
- POST data: POST requests add new resources, the data for which is encoded within the request body.
- Client-side cookies: cookies contain session data about the client, including keys that the server can use to determine their loin status and permissions/accesses to resource.

### HTTP Reponse
HTTP Response status code
- “202 OK” for success
- “404 Not Found” if the source cannot be found
- “403 Forbidden” if the user isn’t authorised to see the resource.
- “301 Moved Permanently” if the file exists but has been redirected to different location. 

> The format of HTTP messages standard: RFC7230  

## Website security 

### threats

- Cross-Site Scripting(XSS): XSS is a term used to describe a class of attacks that allow an attacker to inject client-side scripts through the website into the browsers of other users.
- SQL injection: SQL injection vulnerabilities enable malicious users to execute arbitrary SQL code on a database, allowing data to be accessed, modified, or deleted irrespective of the user’s permissions.
- Cross-Site Request Forgery(CSRF): CSRF attacks allow a malicious user to execute actions using the credentials of another user without that user’s knowledge or consent.
- Clickjacking
- Denial of Service(DoS)
- Directory Traversal
- File Inclusion
- Command Injection

### Methods

- Using more effective password management. Encourage strong passwords that are changed regularly.
- Configure your web server to use HTTPS and HTTP Strict Transport Security(HSTS).
- Keep track of the most popular threats and address the most common vulnerabilities first.
- Only store and display data that you need.


## Web技术

基础：
- HTML, HyperText Markup Language: describe and define the content of a webpage.
- CSS, Cascading Style Sheet: describe the appearance or presentation of content on a webpage.
- HTTP, HyperText Transfer Protocol: deliver HTML and other hypermedia documents on the web.

脚本
- JavaScript: Used to add interactivity and  other dynamic features to your website or application.
- Web API: Used to perform a variety of tasks, such as manipulating the DOM, playing audio or video, or generating 3D graphics.
  - Web API interface reference: Lists all the object types you can use while developing for the web.
  - Web API page: Lists all the communication, hardware access, and other APIs you can use in the web application.
  - Event reference: Lists all the events you can use to track and react to interesting things that have taken place in your webpage or application.
  > Web APIs aretypically used with JavaScript, although this is doesn’t always have to be the case.  
  > Interfaces: types of objects 
- Web Components: A suite of different technologies allowing you to create reusable custom elements - with their functionality encapsulated away from the rest of your code - and utilize them in your web app.允许创建可重用的定制元素（其功能封装在Web代码之外）并在Web应用中使用

图形
- Canvas: The \<canvas> element provides APIs to draw 2D graphics using JavaScript.
- SVG, Scalable Vector Graphics: Use lines, curves, and other geometric shapes to render graphics.
  > With vectors, you can create images that scale cleanly to any size.  
- WdbGL: A JavaScript API that lets you draw 3D or 2D graphics using the HTML <canvas> elements. This technology lets you use standard OpenGL ES in Web content.

音频、视频和多媒体
- Web媒体技术Web media technologies: media-related APIs with links to the documentation.
- 媒体捕捉与流API: Media capture and stream APIs
- 使用HTML音频与视频
- WebRTC(Real-Time COmmunications): Enable audio/video streaming and data sharing between browser clients(peers). 支持浏览器客户端间的对等音频/视频流和数据共享

其他
MathML, Mathematical Markup Language: Display complex mathematical equations and syntax.
XSLT, Extensible Stylesheet language Transformation: Convert XML documents into more human readable HTML.
EXSLT: Extra functions which provide additional features to XSLT.
XPath: Select DOM nodes in a document using a more powerful syntax than what is currently provided by CSS selectors.

## Web development
**Client-side programming**
Improve the appearance and behavior of a rendered web page. This includes selecting and styling UI components, creating layouts, navigation, form validation, etc. 
  
**Server-side programming(back-end scripting)**
Choose which content is returned to the browser in the response to requests.
Validate submitted data and requests, using database to sore and retrieve data and sending the correct data to the client as required.

### Server-side web frameworks
- Django(Python)
- Flask(Python)
- Express(Node.js/JavaScript)
> Node is a browser less environment for running JavaScript  
- Rudy on Rails(Rudy)
- Laravel(PHP)
- ASP.NET(Microsoft, built on the **common language runtime, CLR**)
- Mojolicious(Perl)
- Spring Boot(Java)

**ORM,Object-Relational Mapper**:Web frameworks provide *a database layer* that abstracts database read, write, query, and delete operations.
> Replace the underlying database without necessarily needing to change the code that uses it.  
> Basic validation of data can be implemented within the framework  

### Progressive web apps,PWAs
- Use modern web APIs along with traditional progressive enhancement strategy to create cross-platform web applications.
- Work everywhere and provide several features that give them the same user experience advantages as native apps.
> PWAs are not created with a single technology. They represent a new philosophy for building web apps, involving some specific patterns, and other features.  
**Advantage**
Fire:Fast, Integrated, Reliable, Engaging

- Progressive Enhancement渐进增强认为应该关注内容本身，注意其中的差别。内容是我们建立网站的诱因，有的网站展示它，有的收集它，有的操作等等，但她们都涉及到内容。从内容出发，内容为样式和交互构建起坚实的基础。（内容有丰富寓意化的代码HTML构成，CSS包裹内容，JavaScript是最外层）确保你的标签结构能够将其所包含的内容以最详实的程度传达给用户
- Responsive Design响应式设计，让网站针对不同的浏览器和设备呈现不同的现实效果的策略

> 平稳退化Graceful Degradation的观点认为应该针对哪些最高级最完善的浏览器来设计王振，而将那些背认为“过时”或有功能缺失的浏览器的测试工作安排在开发后期，并把测试对象线定位主流浏览器的前一个版本  

> 媒体查询是强大的工具，使用百分比来布局  
> 使用meta viewport之后可以让布局在移动浏览器上显示的更好  

**Key Principles of a PWA**
- Discoverable: the contents can be found through search engines.
- Installable: it’s available on the device’s home screen.
- Linkable: share it by simply sending a URL.
- Network independent: it works offline or with a poor network connection.
- Progressive:it’s still usable on a basic level on older browser, but fully-functional on the latest one.
- Re-engage-able: it’s able to send notifications whenever there’s new content available. 
- Responsive: it’s usable on any device with a screen and a browser. Responsive web apps use technologies like media queries and viewport to make sure that their UI’s will fit any form factor: desktop, mobile, tablet, or whatever comes next.
- Safe

## Other topics
- Accessibility
- Web performance
- Privacy, permissions, and information security
- Security
- WebAssembly: a new type of code that can be run in modern web browser - it is a low-level assembly-like language with a compact binary format that runs with near-native performance and provides languages such as C/C++ and Rust with a compilation target so that they can run on the web. 

## 网站开发

Tools
- Plain Text editor: Dreamweaver
- Web browser

测试：

**How do you set up a local testing server? **
- Local files: the web address path starts with  file:// followed by the path to the file on your local hard drive.
- Remote files: your examples hosted on GitHub(or some other remote server), the web address will starts with http:// or https://, to show that the file has been received via HTTP. 

**The problem with testing local files**
- They feature asynchronous requests.
- They feature a server-side language: Server-side languages(PHP or Python) require a special server to interpret the code and deliver the result.
**Method: Running a simple local HTTP server**
- Using Python’s SimpleHTTPServer module
1. python -m http.server
2. By default, this will run the contents of directory on a local web server, on port 8000. You can go to this server by going to the URL localhost:8000 in your web browser.Here you will see the contents of the directory listed—click the HTML file you want to run.
> If you already have something running on port 8000, you can choose another port by running the server command followed by an alternative port number, e.g. python -m http.server 7800. You can access your content at localhost:7800.  
3. Running server-side languages locally
*To run Python server-side code*,you’ll need to use a Python web framework(Flask or Django).
*To run Node.js(JavaScript) server-side code*, you’ll need to use raw node or a framework built on top of it.(Express)
*To run PHP server-side code*, launch PHP’s built-in development server
php -S localhost:8000

### Design
### What will your website look like?
- What information does mu website offer?
- What fonts and colors do I want?
> 在写代码之前，做好设计  
### Planning
A website can do basically anything, but for your first try, you should keep things simple.
- What is your website about?
- What information awe you presenting on the subject?
- What foes your website look like, in simple high-level terms? What’s the background color? What king of font is appropriate: formal, cartoony, bold and loud, subtle?
### Sketching out your design
1. Sketch out roughly how you want your site to look.
2. Later on build digital mockups using a graphics editor or web technologies.
> Web teams often include a graphic designer（网站的视觉效果） and a user experience(UX) designer（体验和交互）.  
### Choosing your assets
It’s good to start putting together the content that will eventually appear on your webpage.
- Text
- Theme color: to choose a color, go to the Color Picker and find a color you like #660066 (a hex code, hexadecimal, represents your color)
- Image
- Font
### Dealing with files(text content, code, style sheets, media content etc.)
**Set up a sensible file structure for my website**
> Name folders and files completely in lowercase weigh no spaces. It’s better to separate words with dashes, rather than underscores:my-file.html vs. my_file.html.   
**File structure**
- Index.html: Contain your homepage content, that is, the text and images that people see when they first go to your web site.
- images folder: Contain all the images that you use on your site. 
- styles folder: Contain the CSS code used to style your content.
- scripts folder: Contain all the JavaScript used to all interactive functionality to your site.
> File path  
