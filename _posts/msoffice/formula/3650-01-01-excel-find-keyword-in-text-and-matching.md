---
layout: post
title: "INDEX,MATCH: 주어진 텍스트 안에서 특정 키워드를 찾아 매칭하기"
updated: 2021-08-18
tags: [msoffice,formula]
---

## 키워드 매칭 함수식

어떤 의미인지는 바로 아래 예시를 통해 알아보자. "주소" 와 "시도" 가 목록으로 주어졌을 때, 어떤 주소가 어떤 시도에 해당되는지를 매칭시키고 있다.

![그림00](/img/msoffice/formula/formula-0025.png)

```excel
{= INDEX( 키워드목록, MATCH( true, ISNUMBER( FIND( 키워드목록, 텍스트 )), 0 ))}
```
{:.excel}

위 수식은 중괄호를 제외한 내용을 입력 후, CTRL + SHIFT + ENTER 키를 눌러 **배열수식으로 입력**해야 정상적으로 작동한다. 중괄호는 수식이 배열수식으로 입력되어 있다는 표식으로, 배열수식을 입력한 셀 위에 포커싱을 두면 중괄호 표식이 나온다.

또한 수식을 입력할 땐, 수식을 넣을 셀을 전부 지정하여 배열수식을 입력하는 것이 아니라, 먼저 하나의 셀만 배열수식으로 입력한 뒤, 그 셀을 아래로 복사하여 붙여넣어야 한다.

`키워드목록`은 매칭하고픈 키워드들을 나열한 목록 범위를 가리킨다. 위 그림에서는 "시도" 에 속한 항목들이 키워드목록이다. 그리고 `텍스트`는 어떤 키워드가 담겨있을지 알아보고자 하는 텍스트로 위 그림에서는 "주소" 에 해당한다. 만일 `텍스트` 안의 내용이 `키워드목록`의 어느한가지라도 매칭이 안된다면 #N/A 에러를 발생시킨다.

그리고 **`키워드목록`은 반드시 길이가 긴 단어일수록 위쪽에 위치**해야 한다. 위 그림에서도 4 글자인 시도들이 위쪽에 위치해있다. (이유는 아래에 설명하였다.)

## 함수식 이해

FIND 함수 첫번째 인수를 `키워드목록`과 같은 범위로 지정한 다음 배열수식으로 입력하면, 두번째 인수로 지정한 `텍스트`에 키워드들이 포함되어 있는지를 모두 조사하여 배열로 반환한다.

가장 위쪽에 위치한 키워드부터 먼저 매칭이 되는지 여부를 찾기 때문에, 길이가 긴 키워드부터 `키워드목록`에 지정해줘야만 오류를 방지할 수 있다. 예를들어 위 그림에서 "시도" 에 위치한 키워드 중 "남양주시" 와 "양주시" 위치를 바꾸면 C5, C8, C10 셀의 매칭결과는 "남양주시" 가 아닌 "양주시" 가 된다.

텍스트가 포함되어 있지 않으면 #VALUE! 에러를, 포함되어 있으면 포함된 위치를 숫자로 반환한다. 숫자가 나오는지를 ISNUMBER 함수로 판단하여 가장 먼저 true 값에 해당되는 배열 위치를 INDEX, MATCH 함수로 판단하는 구조다.

마지막으로 참고할 점이 있는데, 여기서는 매칭을 위해 FIND 함수를 사용했지만, 동일한 기능을 수행하는 SEARCH 함수를 사용해도 된다. 다만, FIND 함수와는 다르게 SEARCH 함수는 와일드카드 문자를 허용하기 때문에 오동작할 가능성이 있다. 와일드카드에 대해서는 [별도 포스팅](/post/excel-formula-for-data-cleansing)을 참고해보자.