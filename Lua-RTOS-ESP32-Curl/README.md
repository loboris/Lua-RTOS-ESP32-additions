---

### Net functions implemented using libcurl library

---


#### Included functions

**net.service.curl.get**

GET data from http or https server

   ```
res, hdr,body = net.service.curl.get(url [, fname])

Params:
   url: string; server url, if starting with 'https://' SSL will be used
 fname: optional; if given response body will be written to the file of that name

Returns:
   res: boolean, true on success, false on error
   hdr: received response header or error message on error
  body: received response body or error message on error

Examples:

res, hdr, bdy = net.service.curl.get("http://loboris.eu/ESP32/test.php?par1=765&par2=test")
res, hdr, bdy = net.service.curl.get("http://loboris.eu/ESP32/test.php?name=lobo", "response.txt")
   ```


**net.service.curl.post**

*POST data to http or https server*

   ```
res, hdr,body = net.service.curl.post(url, tparams)

Params:
      url: string; server url, if starting with "https://" SSL will be used
  tparams: Lua table containing post data in form 'key=value'
             special 'key' values are accepted:
               _FILE_key: value is the name of the file to be uploaded; if only "_FILE_" is given, "file" will be used as key
               _JSON_key: value is json string; if only "_JSON_" is given, "json" will be used as key

Returns:
   res: boolean, true on success, false on error
   hdr: received response header or error message on error
  body: received response body or error message on error

Examples:

res, hdr, bdy = net.service.curl.post("http://loboris.eu/ESP32/test.php", {temp=24, user="esp32"})

postreq = {
  temp=24,
  _FILE_file="test.jpg",
  _FILE_file2="data.txt",
  _JSON_jsondata="{\"name\":\"John\", \"age\":31}"
}

res, hdr, bdy = net.service.curl.post("http://loboris.eu/ESP32/test.php", postreq)

Note: More than one file can be uploaded
   ```


**net.service.curl.ftp**

*FTP operations; LIST, GET file, PUT file*

*if the server supports SSL/TLS, secure transport will be used for login and data transfer*

   ```
res, hdr, resp = net.service.curl.ftp(upload, url, user, pass [, fname])

Params:
  upload: if 0, LIST or GET operations are performed; if 1 file upload is performed
     url: string; ftp server url, must strat with "ftp://" or "ftps://"
    user: string; user name to login to ftp server
    pass: string; user passvord to login to ftp server
   fname: optional; string:
                      when upload=0: LIST or file will be written to the file of that name
                      when upload=1: file of that name will be PUT on the server

Returns:
   res: boolean, true on success, false on error
   hdr: received response header or error message on error
  resp: received LIST response or file

Examples:
  NOTE: 'ftp://speedtest.tele2.net' is real ftp server which can be used for testing

-- list files in root directory
res, hdr, resp = net.service.curl.ftp(0,"ftp://speedtest.tele2.net","anonymous","lua.esp32@gmail.com")

-- send file to upload directory (file name MUST be given in URL)
res, hdr, resp = net.service.curl.ftp(1,"ftp://speedtest.tele2.net/upload/image77.jpg","anonymous","lua.esp32@gmail.com","images/myimage.jpg")

-- list files in upload directory to check if file is uploaded (trailing slash MUST be given)
res, hdr, resp = net.service.curl.ftp(0,"ftp://speedtest.tele2.net/upload","anonymous","lua.esp32@gmail.com")

-- get file back under new name
res, hdr, resp = net.service.curl.ftp(0,"ftp://speedtest.tele2.net/upload/image77.jpg","anonymous","lua.esp32@gmail.com","images/newimage.jpg")
   ```


**net.services.curl.sendmail**

*Send email*

*Default SMTP server is 'smtp.gmail.com', it can be changed using net.service.curl.mailserver() function*

   ```
ret,msg = net.services.curl.sendmail(options)

Params:
  options: Lua table containing email options in the following format:
             user=<user_name>        string; login name for SMPT server
             pass=<password>         string; user password for SMTP server
             to=<recipient>          string; recipient's email address
             subj=<subject>          OPTIONAL; string; email subject; DEFAULT: "LUA_RTOS_ESP32"
             msg=<subject>           OPTIONAL; string; email message; DEFAULT: "Message from Lua_RTOS_ESP32"
             secure=<true|false>     OPTIONAL; boolean; if true send using SSL/TLS (DEFAULT: true)
             smtp=<server>           OPTIONAL; string; SMTP server used only for this mail; DEFAULT: use global
             port=<port>             OPTIONAL; integer; SMTP server's port used only for this mail; DEFAULT: use global
             cc=<CC recipient(s)>    OPTIONAL; string or Lua table; CC recipient's email addresses
             attach=<attachment(s)>  OPTIONAL; string or Lua table; file name(s) of the attachment(s)

Returns:
   ret: boolean; true on success, false on error
  stat: string; error message on error, "OK" on success

Examples:

opt = {user="luaesp32@gmail.com", pass="luaESP32password", to="myfriend@gmail.com"}
ret,stat = net.service.curl.sendmail(opt)

opt = {
   user = "luaesp32@gmail.com",
   pass = "luaESP32password",
   to = {"myfriend@gmail.com","otherfriend@gmail.com"},
   subj = "Test",
   msg = "Testing send mail from Lua_RTOS_ESP32",
   secure = true,
   attach = {"picture.jpg", "sd/images/image7.bmp"}
   }
ret,stat = net.service.curl.sendmail(opt)

-- reuse 'opt' for another mail
opt.cc = "forgotten@gmail.com"
opt.subj = "2nd test"
ret,stat = net.service.curl.sendmail(opt)
   ```


**net.services.curl.verbose**

*Set or get verbose level*

   ```
v = net.services.curl.verbose([n])

Params:
  n:  When set to 0 no info is shown,
      when set to 1, verbose information of curl operations is printed; Default: 1
      If not given, current verbose level is returned
      
Returns:
  v:  Verbose level
   ```


**net.services.curl.progress**

*Set or get progress level*

   ```
p = net.services.curl.progres([n])

Params:
  n:  integer (0~20); when set to 0, no progress info is shown,
                      otherwise progress info is shown every n seconds; Default: 1
      If not given, current progress level is returned
      
Returns:
  p:  Progress level
   ```


**net.services.curl.timeout**

*Set or get curl timeout value*

   ```
t = net.services.curl.timeout([n])

Params:
  n:  integer (20~300); curl operation is aborted if it lasts for more than n seconds; Default: 60
      If not given, current timeout value is returned
      
Returns:
  t:  Timeout value in seconds
   ```


**net.services.curl.reclimit**

*Set or get download size limit*

   ```
n = net.services.curl.reclimit([n])

Params:
  n:  integer (< 3MB) Limit for download operations in bytes; Default: 300000
      If not given, current download limit is returned
      
Returns:
  n:  Download limit in bytes
   ```


**net.services.curl.mailserver**

*Set or get SMPT server and port*

   ```
serv, port = net.services.curl.mailserver([serv, port])

Params:
  serv:  string (max len: 64); SMTP server address; Default: "smtp.gmail.com"
  port:  integer (1~65535); SMTP server port; Default: 465
      If not given, current server and port are returned
      
Returns:
  serv:  SMTP server address
  port:  SMTP server port
   ```


**net.services.curl.cleanup**

*Free some memory after using curl*

   ```
net.services.curl.cleanup()
   ```


**net.services.curl.info**

*Get curl version info*

   ```
sv, nv = net.services.curl.info([f])

Params:
  f:  boolean; if true detailed info about curl version and options is printed
      
Returns:
  sv:  string: curl version
  sn:  integer: curl version number
   ```

