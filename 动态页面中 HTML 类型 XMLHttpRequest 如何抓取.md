# 动态页面中 HTML 类型 XMLHttpRequest 如何抓取
#计算机/crawler
## 什么是 XMLHttpRequest
XMLHttpRequest 是一个 API，简称 XHR，它为客户端提供了在客户端和服务器之间传输数据的功能。它提供了一个通过 URL 来获取数据的简单方式，并且不会使整个页面刷新。这使得网页只更新一部分页面而不会打扰到用户。XMLHttpRequest 在 AJAX 中被大量使用。
## 如何抓取 XHR 格式的数据
常见的 XHR API数据返回格式为 json 格式，对获取数据的 url 提交请求后，会返回一个 json 文件。

但有些网站的 XHR 请求数据返回格式为 HTML，即返回一个完整的 HTML 页面文件，但只会刷新页面中的部分内容，在这种情况下，对 url 发出请求后，数据并不会立即返回，而是交由服务端的后台进行处理，直到服务端后台获取完成数据以后，再将获取的数据返回给用户。常见的情况是一些使用了第三方服务或 API 接口的网站，例如电影评分为动态生成的电影票购买网站，它们从豆瓣电影等网站获取实时的电影评分，再将电影评分数据数据返回给客户款，最终生成到页面上。

此时，如果对获取数据的 url 提交请求，返回的状态码会是202 Accepted（请求已经在后台开始处理，但处理还未完成）。要获取最终我们想要的数据，需要持续得对该 url 提交请求，直到最终返回的状态码为200 OK（请求已成功）。一个简单的 python 的例子：

```python
statsu_code = 202
while statsu_code == 202:
    print("Fetching movies info...")
    response = self.session.get(url="https://somedatayouwant.com")
    statsu_code = response.status_code
print(response.content)
```
