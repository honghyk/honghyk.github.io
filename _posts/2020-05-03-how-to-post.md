---
layout: post
title: Jekyll로 포스팅하기
tags: [jekyll]
comments: true
---

Jekyll을 이용해서 블로그에 첫번째 포스트를 써보자.


Jekyll을 이용해서 블로그에 포스팅을 올리려면 `_post` 디렉토리에 `YEAR-MONTH-DAY-title.md` 형식으로 파일명을 작성해야 한다. 이 형식으로 작성된 마크다운 파일만이 Jekyll에서 인식하는 올바른 포스팅 파일이다.  

여기서 `YEAR`는 네 자리 숫자, `MONTH`와 `DAY`는 두 자리 숫자이다. 그래서  올바른 파일명은 다음과 같다.  
```
2015-08-16-code-highlighting-post.md
2016-08-14-style-test-ko.md
```

<br>모든 블로그 포스트 파일은 YAML 머리말 블록을 포함하고 있어야 하는데, 내용이 꼭 있어야 할 필요는 없다. 삼중 대쉬 행 사이를 비워 놓기만 해도 된다.  
YAML 머리말은 글의 레이아웃, 제목, 태그, 카테고리, 날짜 등을 설정 하는데 사용된다.  


지금 쓰고 있는 kiko-now 테마에서는 `layout`, `title`, `tags`, `comments` 정도만 지정해 줘도 될 듯 하다.
```
---
layout: post
title: Jekyll로 포스팅하기
tags: [jekyll]
comments: true
---
```
`layout`에는 지금 쓰고 있는 테마에서는 post를 제외하고는 쓸 일이 없을 듯 하다.  
`title` 에는 제목, `tags` 에는 [ ]안에 원하는 태그, `comments` 는 댓글 기능을 활성화 해놓지 않아서 딱히 상관 없을 듯 하다.<br><br>



## 초안(draft) 작성하기
아직 포스팅 하고 싶지 않은 초안은 루트 디렉토리에 `_drafts` 폴더를 만들고 그 안에 생성하면 된다. 초안이기 때문에 파일명에 날짜를 넣어줄 필요는 없다. 초안을 포함해서 사이트 미리보기를 하려면 다음 명령을 실행하면 된다.
```
bundle exec jekyll serve --drafts
``` 

그러면 초안의 작성 시간에 해당 파일의 수정 시간을 할당하기 때문에, 다음과 같이 현재 작업중인 초안이 가장 최신 포스트인 것 처럼 표시되는 것을 볼 수 있다.
![이미지 가운데 정렬](/images/draft.png){: .center-image}
