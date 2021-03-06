## [CH6] HTTP 5

```๐  ์ค๋ ๋ฐฐ์ธ ๋ด์ฉ ```

    1. HTTP Cache๋?
    2. Cache๊ด๋ จ Header = Cache Control, Expired, ETag, LAST-MODIFIED
----------

## **1. HTTP์ Cache๋?**

* ์น์ฌ์ดํธ๋ฅผ ํตํด ์ด๋ฏธ์ง , js, html ํ์ผ ๋ฑ์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ง๊ณ  ์ฌ ๋ ํด๋น ๋ฐ์ดํฐ์ ํฌ๊ธฐ๋งํผ์ ํต์  ๋ฐ์ดํฐ ์ฒ๋ฆฌ๊ฐ ํ์ํ๋ค. 
* ๋์ผํ ์ด๋ฏธ์ง๋ฅผ ์ ์ํ  ๋ ๋ง๋ค ๋ฐ์์จ๋ค๋ฉด ํด๋ผ์ด์ธํธ ์์ฅ์์๋ ๋ถ๋ด์ด๋ฉฐ ์ฌ๋ฌ ํด๋ผ์ด์ธํธ๋ฅผ ๋์์ ์๋ํ๋ ์๋ฒ์๋ ๋๋์ฑ ๋ถ๋ด์ด ๋๋ค.
* ์ด๋ฌํ ๋ฌธ์ ์ ์ผ๋ก HTTP์์๋ **์บ์ฑ**์ ์ง์ํ๋ค.
* ๐ ์ฆ, Resource file ์์ ์ ์ฅํ์ฌ ๋์ผํ ์น ์ฌ์ดํธ ์ ์ ์ ๋ก๋ฉ ์๊ฐ์ ์ค์ฌ์ค๋ค

### **1.1 HTTP ์บ์ฑ์ด๋?**
* ์ผ๋ฐ ์ ์ผ๋ก ์บ์ฑ์ GET ์์ฒญ์์ ์ฒ๋ฆฌํ๋ค.
* HTTP ์บ์ฑ์ *Browser Caches(์ฌ์ค ๋ธ๋ผ์ฐ์  ์บ์)*์ *๊ณต์  ์บ์* ๋ ๊ฐ์ง ์ข๋ฅ๊ฐ ์๋ค.

### **Browser Caches(์ฌ์ค ๋ธ๋ผ์ฐ์  ์บ์)**

![์ฌ์ค ๋ธ๋ผ์ฐ์  ์บ์](./image/privateCache.png)<br> 
```๊ฐ Client์ ๋ก์ปฌ ์บ์์ ๋ฐ์ดํฐ๋ฅผ ์ ์ฅํ๋ ๊ฒ์ผ๋ก ๋ธ๋ผ์ฐ์  ์บ์๋ฅผ ์ฃผ๋ก ์ด์ฉ```

* ์ฌ์ค ๋ธ๋ผ์ฐ์  ์บ์๋ **๊ฐ์ธ ์ ์ฉ**์ผ๋ก ์ฌ์ฉํ๋ ์บ์์ด๋ค. 
* ์ ์ฅ๋ ์บ์๋ ์๋ฒ์์ ์ถ๊ฐ๋ก ๋ฐ์ดํฐ๋ฅผ ๋ฐ์์ค์ง ์๊ณ  ๋ฐ๋ก ์ฌ์ฉํ  ์ ์๊ฒ ํด์ค๋ค.
* ๋ธ๋ผ์ฐ์ ์ Back๋ฒํผ ๋๋ ์ด๋ฏธ ๋ฐฉ๋ฌธํ ํ์ด์ง๋ฅผ ์ฌ ๋ฐฉ๋ฌธํ๋ ๊ฒฝ์ฐ ๊ทน๋ํ

### **Proxy Caches(๊ณต์  ํ๋ก์ ์บ์)**

![๊ณต์  ํ๋ก์ ์บ์](./image/ShareCache.png)<br> 
```๋ธ๋ผ์ฐ์ ์ ์๋ฒ ์ฌ์ด์ ์๋ ์บ์๋ก ๋ฆฌ๋ฒ์ค ํ๋ก์ค, ๊ฒ์ดํธ์จ์ด, CDN ๋ฑ๋ฑ์ ์ ์ฅ๋๋ ์บ์ ๐์ฆ, Client๋ Server๊ฐ์๋ ๋คํธ์ํฌ ์์์ ๋์```

* ์ฌ์ค ๋ธ๋ผ์ฐ์  ์บ์๋ ์บ์ฑํ๊ธฐ ์ํด์ ๋ชจ๋  Client๊ฐ ํ๋ฒ์ฉ์ ์ค์  ๋ฆฌ์์ค๋ฅผ ์ ๊ทผํด์ผ ํ๋ค ๐ ๊ณต์  ํ๋ก์ ์บ์๋ ์๋ฒ์ ํด๋ผ์ด์ธํธ ๊ฐ์ด๋ฐ ํ๋ก์ ๋ฑ์ ํตํด ์บ์๋ฅผ ํ๋ ๊ฒ์ด๋ค.

* ๐ [์์๋ฅผ ๋ค์ด ์ดํดํ๊ธฐ] <br>
    [์ฌ์ค ๋ธ๋ผ์ฐ์  ์บ์] 10๋ง๋ช์ด ์ ์ํ๋ฉด, ์ค์  ๋ฆฌ์์ค์ 10๋ง๋ฒ ์ ๊ทผ ํ์<br>
    [๊ณต์  ํ๋ก์ ์บ์] 10๋ง๋ช ์ ์ํ๋ฉด, ์ค์  ๋ฆฌ์์ค์๋ 1๋ฒ ์ ๊ทผ(ํ๋ก์์ ์ ์ฅํ๊ธฐ ์ํด), ๋๋จธ์ง 9๋ง 999๋ฒ์ ๊ณต์  ์บ์์์ ์ฒ๋ฆฌ

### **1.2 HTTP ์บ์ฑ์ ์ฅ์ **
1. ๋ถํ์ํ ๋ฐ์ดํฐ ์ ์ก์ ์ค์ฌ ๋คํธ์ํน ๋น์ฉ์ ์ค์ฌ์ค๋ค.
2. ๊ฑฐ๋ฆฌ๋ก ์ธํ ์ง์ฐ์๊ฐ์ ์ค์ฌ ์นํ์ด์ง๋ฅผ ๋นจ๋ฆฌ ๋ถ๋ฌ์ฌ ์ ์๊ฒ ๋๋ค.
3. ์๋ฒ์ ๋ํ ์์ฒญ์ ์ค์ฌ ์๋ฒ์ ๋ถํ๋ฅผ ์ค์ธ๋ค.

----------
## **2. Cache๊ด๋ จ Header**

๐ ๋ธ๋ผ์ฐ์ ๋ ํ๋ฒ ์์ฒญํ ํ์ผ์ ๊ทธ ์ดํ๋ถํฐ Cache์ ์ ์ฅํด์ ์ฌ์ฉํ๋ค. ๊ทธ๋ ๋ค๋ฉด Cache๋๋ฉด ์๋๊ฑฐ๋ ์บ์ฌ๋ ๋ด์ฉ์ด ๋ณ๊ฒฝ๋๋ฉด ์ด๋ป๊ฒ ํด์ผ ๋ ๊น?

๐ Cache-Control์ ์ด์ฉํด Cache ์ปจํธ๋กคํ๋ฉด ๋๋ค.

### **2.1 Cacheability-Cache ์ ์ด**

| - | Cacheability |
|:---:|---|
|public|์บ์ ํ์ฉ. ์๋ต์์๋ค์ ์๋์ผ๋ก private์ด ๋๋ค.|
|private| ํน์  ์ ์ (์ฌ์ฉ์์ ๋ธ๋ผ์ฐ์ )๋ง ์บ์ฌ ํ๋๋ก ์ค์ ํ๋ค. ์ฌ๋ฌ ์ฌ๋์ด ์ฌ์ฉํ๋ ๋คํธ์ํฌ์์ ์ค๊ฐ์ ์ญํ ์ ํ๋ shared caches (์: proxy) ์๋ ๊ฒฝ์ฐ ์บ์ฌ๋์ง ์์ต๋๋ค.|
|no-store|์ด๋ค ์ํฉ์์๋ ํด๋น reponse ๋ฐ์ดํฐ๋ฅผ ์ ์ฅํ์ง ์๋๋ค(๋ถ์ฃผ์ํ๊ฒ ๊ณต๊ฐ๋์๊ฑฐ๋ ๋ฏผ๊ฐํ ์ ๋ณด์ ์ ์ฅ์ ๋ง๋ ๊ฒ)|
|no-cache|์บ์๋ ๋ณต์ฌ๋ณธ์ ์ฌ์ฉ์์๊ฒ ๋ณด์ฌ์ฃผ๊ธฐ ์ด์ ์, ํญ์ ์ฌ๊ฒ์ฆ์ ์ํ ์์ฒญ์ ์๋ฒ๋ก ๋ณด๋ด๋๋ก ํ๋ค(์์ฒญ์ ํ ๋ ๋ง๋ค ํ์ธ์ ํ๊ณ  ์์ ๋์์๋๋ง ๋ค์ด๋ก๋๋ฅผ ์งํํ๊ณ  ์ถ์๋)|

[๐ ์ฐธ๊ณ ] public๊ณผ private๋ ์ฌ์ค ์บ์์ ์ ์ฅํ  ์ง or ๊ณต์  ์บ์๋ก ์ ์ฅํ ์ง ์ง์ ํ  ๋ ์ฌ์ฉํ๋ ์ต์

### **2.2 Expire**

| - | Expire |
|:---:|---|
|max-age|์บ์ ์์ฑ์ ์บ์์ ๋ง๋ฃ๊ธฐ๊ฐ์ ์ ํด๋๋ ๊ฒ. ์ง์ ๋ ๋ง๋ฃ์ผ์ด ์ง๋๋ฉด ์บ์๋ฅผ ์ญ์ ํ๊ณ  ๋ค์ ์บ์๋ฅผ ์์ฑํ๋ ์ต์|

๐ Cache-Control : max-age=86500<br>
    => ์๋ต์ ์ต๋ 1์ผ (60 * 60 * 24 = 86500)๋์ ๋ธ๋ผ์ฐ์  ๋ฐ ์ค๊ฐ ์บ์๊ฐ ์บ์ํ  ์ ์๋ค(public)

๐ Cache-Control : private, max-age=600<br>
=> ์๋ต์ ์ต๋ 10๋ถ (60 * 10)๋์ ํด๋ผ์ด์ธํธ์ **๋ธ๋ผ์ฐ์ **๋ง ์บ์ฑ ๊ฐ๋ฅํ๋ค.

๐ Cache-Control : max-age=15<br>
=> ์๋ต์ 15์ด ๋์ ์บ์์ ์ ์ฅํ๋ค. ์ด๋ 15์ด ๋์ ์ฌ๋ฌ๋ฒ Request์ ํ๋๋ผ๋, request-response ๋์ ์ํ๋ค => ๋ด๋ถ์ ์บ์๋ ๋ฐ์ดํฐ๋ฅผ ๋ฐ๋ก ์๋ต์ผ๋ก ๋ฐ์๊ธฐ ๋๋ฌธ์ด๋ค.

### **2.3 Cache ๋์ ๊ณผ์ **

#### **1. ์ฒซ ์์ฒญ**
1. ๋ธ๋ผ์ฐ์ ๋ ์๋ฒ์ Index.html ํ์ผ์ ์์ฒญํ๋ค.
2. ์๋ฒ๋ index.htmlํ์ผ์ ์ฐพ์๋ณด๊ณ  ์กด์ฌํ๋ ํ์ผ์ด๋ฉด ํ์ผ ๋ด์ฉ๊ณผ ํค๋์ ํจ๊ป ๋ธ๋ผ์ฐ์ ์๊ฒ ์๋ตํ๋ค.
3. ๋ธ๋ผ์ฐ์ ๋ ์๋ต ๋ฐ์ ๋ด์ฉ์ ๋ฐ๋ผ Cache ์ ์ฑ์ ์ํํ๋ค.<br>
   ๐ ์๋ต ํค๋์ Last-Modified, Etag, Expires, Cache-Control : max-age ํญ๋ชฉ์ด ์กด์ฌํ๋ฉด ์ด๋ฅผ ๋ณต์ฌํด ๊ฐ์ ์ ์ฅํ๋ค.

#### **2. ์ฌ์์ฒญ**

##### [1] LAST-MODIFIED

![asda](./image/HTTPCache1.png)<br>

1. ๋ธ๋ผ์ฐ์ ๋ ์ต์ด ์๋ต ๋ ๋ฐ์ Last-Modified ๊ฐ์ If-Modified-Since๋ผ๋ ํค๋์ ํฌํจ์์ผ, ํ์ด์ง๋ฅผ ์์ฒญํ๋ค.
2. ์๋ฒ๋ ์์ฒญ ํ์ผ์ ์์  ์๊ฐ์ If-Modified-Since๊ฐ๊ณผ ๋น๊ตํ๋ค. 
3. ์ด๋ ๋์ผํ๋ฉด **304 Not Modified**๋ก ์๋ตํ๊ณ , ๋ง์ฝ ๋ค๋ฅด๋ค๋ฉด **200 OK**์ ํจ๊ป ์๋ก์ด Last-Modified ๊ฐ์ ์๋ต ํค๋์ ์ ์กํ๋ค.
4. ๋ธ๋ผ์ฐ์ ๋ ์๋ต ์ฝ๋๊ฐ **304**์ด๋ฉด Cache์์ ํ์ด์ง๋ฅผ loadํ๊ณ , **200**์ด๋ฉด ์๋ก ๋ค์ด ๋ฐ์ ํ Last-Modified ๊ฐ์ ๊ฐฑ์ ํ๋ค.

##### [2] ETag

* ETag : HTTP ์ปจํ์ธ ๊ฐ ๋ฐ๋์๋์ง๋ฅผ ๊ฒ์ฌํ  ์ ์๋ ํ๊ทธ(์ด๊ฑด  LAST-MODIFIED์ฒ๋ผ ์๊ฐ์ ๊ธฐ๋ฐ์ด ์๋๋ผ Token ๊ฐ์ ์ด์ฉํ ๊ฒ์ด๋ค)

[๐ ์ฐธ๊ณ ] ์ฌ์ค, ์ LAST-MODIFIED๋ง ๊ฐ์ง๊ณ  ์ ํจ์ฑ์ ์ฒดํฌํ๊ธฐ์๋ ์ถฉ๋ถํ์ง ์๋ ๊ฒฝ์ฐ๊ฐ ์กด์ฌํ๋ค.

        1. ๋ฌธ์๊ฐ ์ฃผ๊ธฐ์ ์ผ๋ก ์๋ฐ์ดํธ๋ ๋์๋๋ฐ, ๋ฐ๋ ๋ด์ฉ์ด ์์
        2. ๋ณ๊ฒฝ๋ ๋ด์ฉ์ ์๋๋ฐ, ์๋ต์์ผ๋ก ์๋ฏธ๊ฐ ํฌ์ง ์๋ ๊ฒฝ์ฐ (์ฃผ์ ๋ณ๊ฒฝ, ์ฒ ์ ๋ณ๊ฒฝ)
        3. ์ ํจํ ์๊ฐ์ ์ ํ๊ธฐ ์ด๋ ค์ด ๊ฒฝ์ฐ
        (์ด๋ค ์๋ฒ๋ ์์ ์ด ๊ฐ์ง๊ณ  ์๋ ํ์ด์ง์ ๋ํ ์ต๊ทผ ๋ณ๊ฒฝ์ผ์ ์ ๋ชจ๋ฅธ๋ค)

์ด๋ด ๊ฒฝ์ฐ์, ETag๊ฐ ๊ฐ์ง ์์ ๊ฒฝ์ฐ์๋ง  ์ ๊ฐ์ฒด๋ฅผ ๋ฌ๋ผ๊ณ  ์์ฒญํ๋ค.

![asda](./image/HTTPCache2.png)<br>

1. ๋ธ๋ผ์ฐ์ ๋ ์ต์ด ์๋ต ์ ๋ฐ์ ETag ๊ฐ์ If-None-Match๋ผ๋ ํค๋์ ํฌํจ์์ผ ํ์ด์ง๋ฅผ ์์ฒญํ๋ค.
2. ์๋ฒ๋ ์์ฒญ ํ์ผ์ ETag๊ฐ์ ์๋ฒ ๋ฆฌ์์ค์ ETag๊ฐ๊ณผ ๋น๊ตํ๋ค. 
3. ์ด๋ ๋์ผํ๋ฉด **304 Not Modified**๋ก ์๋ตํ๊ณ , ๋ง์ฝ ๋ค๋ฅด๋ค๋ฉด **200 OK**์ ํจ๊ป ์๋ก์ด Last-Modified ๊ฐ์ ์๋ต ํค๋์ ์ ์กํ๋ค.
4. ๋ธ๋ผ์ฐ์ ๋ ์๋ต ์ฝ๋๊ฐ **304**์ด๋ฉด Cache์์ ํ์ด์ง๋ฅผ loadํ๊ณ , **200**์ด๋ฉด ์๋ก ๋ค์ด ๋ฐ์ ํ ETag ๊ฐ์ ๊ฐฑ์ ํ๋ค.

##### [3] Cache-Control

![asda](./image/HTTPCache3.png)<br>

1. ๋ธ๋ผ์ฐ์ ๋ ์ต์ด ์๋ต ์ ๋ฐ์ Cache-Control ์ค max-age๊ฐ(์ด ๋จ์)๋ฅผ GMT์ ๋น๊ตํ์ฌ ๊ธฐ๊ฐ ๋ด๋ผ๋ฉด ์๋ฒ๋ฅผ ๊ฑฐ์น์ง ์๊ณ  ์บ์ฌ์์ ํ์ด์ง๋ฅผ ๋ก๋ ํ๋ค. ๋ง์ฝ ๊ธฐ๊ฐ์ด ๋ง๋ฃ๋์๋ค๋ฉด validation ์์์ ์ํํ๋ค.
