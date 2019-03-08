SSE is a technology where a browser receives automatic updates from a server via an HTTP
connection. The SSE API is standardized as a part of HTML5 by the W3C. SSE is used to send
message updates or continuous data streams to a browser client. It needs to be manually
terminated, otherwise, it will keep on running until an error occurs.
Five of the RESTful APIs are using SSE: they are scan (chapter 5.3.1), get device connection
status (chapter 5.3.7), receive indication and notification (chapter 5.3.8), monitor Cassia
router’s status (chapter 5.2.3) and create combined SSE (chapter 5.6.1, firmware 1.3).
Each SSE response starts with “data:”. When debugging, you can input the URL of an SSE
into a web browser, then you will see the SSE output from the web browser.
In the program, an SSE request won’t return any data if you call the interface like a normal
HTTP request, because a normal HTTP request only returns output when it finishes. In
addition, when calling an SSE, you should monitor this thread. If it is interrupted by an error
or any unexpected incident, you can restart it.

**NOTE**: If you use tools like CURL for HTTP request, the tool will return data when the HTTP
request ends. However, SSE API which NEVER ends and sends data in a stream, so it will
hang the page. You should add the following snippet (use scan as an example):

#### PHP
```php
if ($stream = fopen($url_for_scan, 'r')) {
  while(($line=fgets($stream))!== false){
    echo $line;
  }
}
```
