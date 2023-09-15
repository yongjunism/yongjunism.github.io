---
layout: post
title: "구글에 깃허브 블로그(github.io) 노출시키기"
subtitle: "How to expose github blog to Google"
category: tips
tags: blog github-pages google-search-console
---

개발 블로그의 목적에는 나만 보려고 개인적으로 기록물을 남기기 위함도 있겠지만,<br>
그 내용을 남들과 공유할 때 빛을 발할 수 있겠죠!<br>

하지만 Github Pages로 만든 깃허브 블로그는 별도의 설정을 해주지 않으면 구글 검색엔진에 잘 노출되지 않습니다.<br>
남들이 만든 깃허브 블로그는 잘만 보이던데, 어떻게 하면 될까요?<br>

바로 [Google Search Console]에 등록하면 됩니다.<br>

## [Google Search Console]
[Google Search Console]은 구글에서 제공하는 서비스로,<br>
구글 검색결과에서 사이트를 돋보이게 할 수 있고, 나아가 검색 트래픽 및 실적을 확인할 수 있습니다.<br>

![Google Search Console](/assets/img/2023-04-01/google_search_console.png)
위 링크를 들어가서 **시작하기**를 클릭합니다.<br>

![Google Search Console 2](/assets/img/2023-04-01/google_search_console2.png)
도메인이 있으면 좌측, 도메인 없이 지정된 url이 있으면 우측의 **URL 접두어**를 선택합니다.<br>
저는 깃허브에서 제공하는 기본 url(github.io)를 그대로 사용하므로 우측으로 진행했습니다.<br>

![Google Search Console 3](/assets/img/2023-04-01/google_search_console3.png)
유효한 URL인지 확인되면 소유권을 확인하는 절차가 있습니다.<br>
- **HTML 파일: 웹사이트에 HTML 파일 업로드(권장)**
- HTML 태그: 사이트 홈페이지에 메타태그 추가
- Google 애널리틱스: Google 애널리틱스 계정 사용
- Google 태그 관리자: Google 태그 관리자 계정 사용
- 도메인 이름 공급업체: DNS 레코드와 Google 연결

총 5가지 방법이 존재하지만, **HTML 파일**로 진행합니다.<br>
다운로드받은 파일을 블로그 폴더의 루트 경로에 넣어줍니다.<br>

소유권 확인만 됐을 뿐, 아직 Google 크롤러가 우리 블로그의 어떻게 크롤링 할줄 모릅니다.<br>
크롤러가 열심히 일할 수 있도록 약간의 설정을 해줍시다.<br>

### sitemap.xml 생성
```html
---
layout: null
---

<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd"
        xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    {% for post in site.posts %}
    <url>
        <loc>{{ site.url }}{{ post.url }}</loc>
        {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
        {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
        {% endif %}

        {% if post.sitemap.changefreq == null %}
        <changefreq>weekly</changefreq>
        {% else %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
        {% endif %}

        {% if post.sitemap.priority == null %}
        <priority>0.5</priority>
        {% else %}
        <priority>{{ post.sitemap.priority }}</priority>
        {% endif %}

    </url>
    {% endfor %}
</urlset>
```
sitemap.xml 파일을 블로그 폴더의 루트 경로에 생성합니다.<br>
위 코드를 복사, 붙여넣기하고 저장합니다.

## robots.txt 생성
```
User-agent: *
Allow: /

Sitemap: https://yongjunism.github.io/sitemap.xml
```
앞으로 Google 크롤러는 robots.txt에 적힌 사이트 주소와 크롤링 정책을 바탕으로<br>
우리 블로그의 내용을 수집하게 됩니다.

## sitemap.xml 제출
![Google Search Console 4](/assets/img/2023-04-01/google_search_console4.png)
좌측 Sitemaps 탭에서 우리의 sitemap.xml을 등록합시다.<br>
아마 처음 등록할 때는 바로 '성공'으로 뜨지 않을텐데요.
'사이트맵을 읽을 수 없음', '실패' 등등의 문구를 마주하실 확률이 높습니다.<br>
앞서 진행한 절차에 문제가 있는 것은 아니니, 이제 며칠 간 인내심을 갖고 기다리시면 됩니다.<br>

자, 이제 모든 과정이 끝났습니다.<br>
저처럼 **성공**이라는 문구를 마주쳤다면 Google에 잘 검색될텐데요.<br>
앞으로 신나게 글을 포스팅하시면 되겠습니다.<br>

[Google Search Console]: https://search.google.com/search-console/about