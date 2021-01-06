# express

설치:

npm install express를 터미널에 치면

dependency에 express가 추가된다.



## require	 

```javascript
const express = require('express');
```

여기보면 express라는 상수에 require('express')를 넣어주고있다.

그리고 강의에선 그냥 넘어가더라.

require()는 자바스크립트의 함수가 아니다.

이것은 node.js의 함수이다.

require는 모듈을 로드하는데에 쓰인다.

모듈은 쪼마난 프로그램 이라고 보면된다.

c에서 include,python에서 import하는 라이브러리? 뭐 그런느낌.세세하게는 다르지만, 기본개념은 비슷하다.일부 기능만 구현해논거를 우리가 가져다가 쓰는거다.

바닐라 js에서는 script를 src로 받아와서 하지만, node.js에서는 모듈을 require해서 쓴다.스크립트와는 달리 모듈은 그 자체로 독립적이다!



### npm

npm은 자바스크립트 모듈들을 호스트해주느 서비스이다.npm install express를 통해 express 모듈을 우리의 프로젝트의 node_modules폴더에다 다운받아준다.

우리는 이걸 require로 가져와서 쓰면 된다~



## express()

```javascript
const app = express()
```

express를 require해서 모듈을 얻어냈다(정확히는 인스턴스를 만들수있는 함수를 얻어냄).

express()를 통해 새로운 app을 만들어준다.만약 여러번 express()해서 app1, app2..를 만든다면, 여러개를 동작할 수도 있따.



## listen

```
app.listen(portnum,callbakfun)을 통해 서버를 호스트 할 수 있다. 저걸 실행하면 입력한 포트넘버로의 request를 listen하는 서버가 만들어지고 callbackfun이 실행된다.
```

localhost:3000으로 3000번 포트에 접근할 수 있다.

## without express

내 생각에 express는 그저 모듈일 뿐이므로

당연하게도 안쓰고 내가 직접 만들 수 있다.

검색해봤더니 다음코드로 가능하단다.

```javascript
var http = require('http'); //http를 require한다?
var fs = require('fs'); //fs를 require한다?fs는 filesystem이라고 한다.
var path = require('path');//파일주소를 다루는 모듈이란다.

http.createServer(function (request, response) { //http를 통해 서버를 만든다.
    console.log('request ', request.url);

    var filePath = '.' + request.url;
    if (filePath == './')
        filePath = './index.html';
    
    var extname = String(path.extname(filePath)).toLowerCase();//url을 만드는 과정같음
    var contentType = 'text/html';
    var mimeTypes = {
        '.html': 'text/html',
        '.js': 'text/javascript',
        '.css': 'text/css',
        '.json': 'application/json',
        '.png': 'image/png',
        '.jpg': 'image/jpg',
        '.gif': 'image/gif',
        '.wav': 'audio/wav',
        '.mp4': 'video/mp4',
        '.woff': 'application/font-woff',
        '.ttf': 'application/font-ttf',
        '.eot': 'application/vnd.ms-fontobject',
        '.otf': 'application/font-otf',
        '.svg': 'application/image/svg+xml'
    };

    contentType = mimeTypes[extname] || 'application/octet-stream';

    fs.readFile(filePath, function(error, content) {
        if (error) {
            if(error.code == 'ENOENT'){
                fs.readFile('./404.html', function(error, content) {
                    response.writeHead(200, { 'Content-Type': contentType });
                    response.end(content, 'utf-8');
                });
            }
            else {
                response.writeHead(500);
                response.end('Sorry, check with the site admin for error: '+error.code+' ..\n');
                response.end();
            }
        }
        else {
            response.writeHead(200, { 'Content-Type': contentType });
            response.end(content, 'utf-8');
        }
    });
//로컬에서 파일을 찾아주는거같음..
}).listen(8125);//여기 listen으로 포트를 다루고 있다..listen의 원리는 나중에 네트워크를 배울때 나오지 않을까 싶다..지금온 도저히 모르겠음.
console.log('Server running at http://127.0.0.1:8125/');
```

 ## /

/는 root를 표시한다.

google은 https://www.google.com.

위의 경우에는 localhost:3000



## get

listen을 쓰면 브라우저가 localhost:3000에 접근할 수는 있다. 그러나 아무것도 response받지 않았으므로 내용이 뭐 비어있거나 에러메시지가 있거나 할것이다.

```
app.get(requstlocation,callbackfun)을 통해 포트에 request를 get했을 경우 response할 수 있따.
```

```javascript
app.get("/",function(request,response){
    response.send("hello");
})
```

참고로 저기에 function의 인자로 들어가는 request는 get함수를 해체해보면 내부에서 만들어주고 있을거다.아니면 뭐 딴데서 받아왔을라나..

request에는 뭐 어떤 url로 접근했는지 등등 엄청난 정보가 담겨있다!

포트에 request가 돌아오면 콜백이 실행될거고 hello문자열을 보낼거다!

저 안의 문자열은 html태그도 가능하다.

sendFile을 통해 파일을 보낼수도 있다.