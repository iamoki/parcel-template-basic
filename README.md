# ParcelBundler Setting Process

>ParcelBundler를 사용해 기본적인 세팅하기

## **Step1.**
package.json파일 생성

~~~bash
npm init -y
~~~

개발의존성 패키지로 parcel bundler 설치
~~~bash
npm i -D parcel-bundler
~~~

dev스크립트를 통해 parcel 명령을 실행하고 index.html을 연결하고,  
build스크립트를 통해 index.html을 실행
~~~json
"scripts": {
    "dev": "parcel index.html",
    "build": "parcel build index.html"
  },
~~~
---
## **Step2.**
scss폴더와 js폴더를 만들어 연결
~~~html
<link rel="stylesheet" href="./scss/main.scss" />
<script defer src="./js/main.js"></script>
~~~
---
## **Step3.**
❌ <u>favicon이 루트경로에 있는데도 설정 안되는 이유</u>  
✅ parcel-bundler를 통해 dist폴더로 변환되어 삽입되는데 그 경로에 파비콘 파일이 없기 때문.

☀️ **해결방법**  
>favicon을 dist 폴더에 넣어준다. 하지만 dist폴더는 언제나 개발서버실행을 통해서 생성하고 지워지는게 반복되므로 직접 사용하는 파일을 삽입하는 것은 좋은 방법이 아니다. 그러므로 해당 파일(파비콘)을 개발서버를 열거나 제품화 시킬때 dist폴더에 자동으로 삽입될 수 있도록 패키지의 도움을 받는다.  

[parcel plugin static files copy](https://www.npmjs.com/package/parcel-plugin-static-files-copy) 개발 의존성 모드로 설치
~~~bash
npm i -D parcel-plugin-static-files-copy
~~~

package.json 설정
~~~json
"staticFiles": {
    "staticPath": "static"
}
~~~
루트경로에 static폴더를 생성후 그안에 파비콘 넣기.

***
## **Step4.**
postcss의 autoprefixer패키지를 사용해 접두사 자동으로 붙여주기
~~~bash
npm i -D postcss autoprefixer
~~~

package.json에 브라우저 지원범위 명시(점유율이 1% 이상인 모든 브라우저의 마지막 2개 버전까지 지원하겠다는 의미)
~~~json
"browserslist": [
    "> 1%",
    "last 2 versions"
]
~~~

.postcssrc.js 파일 설정
~~~javascript
// 변수에 담아 설치한 autoprefixer 가져오기
const autoprefixer = require('autoprefixer')

// autoprefixer 내보내기
module.exports = {
    plugins: [
        autoprefixer
    ]
}
~~~

에러가 난다면?(에러문구:PostCSS plugin autoprefixer requires PostCSS 8.) - autoprefixer과 postcss의 버전충돌

해결방법  
autoprofixer의 버전을 9~버전으로 낮춰 postcss와 호환되게 해준다.
~~~bash
npm i -D autoprefixer@9
~~~
---
## **Step5.**
Babel 적용  
@babel/core @babel/preset-env 두개 패키지 설치
~~~bash
npm i -D @babel/core @babel/preset-env
~~~
최신 JS문법이 바벨에 의해 변환이 안될 때 추가 설치
~~~bash
npm i -D @bebal/plugin-transform-runtime
~~~
.babel.js 파일 설정
```javascript
module.exports = {
    // 이를 설정함으로써 작성된 JS문법들이 ES5 문법으로 변경가능
    presets: ['@babel/preset-env'],
    // 추가로 설치한 플러그인 설정 
    plugins: [
        ['@babel/plugin-transform-runtime']
    ]
}
```


