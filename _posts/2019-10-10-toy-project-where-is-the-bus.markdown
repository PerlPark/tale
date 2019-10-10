---
layout: post
title: "토이 프로젝트: 버스야 어디있니? 진행 상황 정리"
date: 2019-10-10 21:55:55 +0900
description: # Add post description (optional)
img: # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Project] # add tag
---

포폴용으로 쓰기 위해 열심히 만들고 있는 프로젝트다. Nodejs, API는 이번 기회에 처음 사용해봤는데, 자바스크립트를 알고나니까 생각보다 낯설진 않았다. 하지만 이제 막 배워가면서 만들다보니 뭔가를 하나 만들면 오류도 하나 생겨서 디버깅에 시간을 엄청 쓰고있다. 코드도 정리되어야 할 부분이 너무 많다. ㅠㅠ 그래도 기본적으로 생각했던 기능들은 구현되어서 좀 정리를 해보려고 포스팅을 해 본다.

### 사용자 위치 좌표 조회
1. HTML5 Web API > geolocation API를 사용해서 사용자의 좌표를 조회한다.
2. Kakao 지도 API로 좌표를 도로명 주소로 변환하여 출력한다.
3. 1번 과정에서 얻어낸 좌표는 localStorage에 저장된다. 화면이 갱신되면 API 응답 기다리는 동안 우선 출력하기 위함이다. 
4. 폼 제출 시 좌표 값을 전달하기 위해, hidden 인풋(lat, lng)에도 좌표 값이 저장된다.

[<img src="{{site.baseurl}}/assets/post_img/toy-project-where-is-the-bus-1.png">]({{site.baseurl}}/assets/post_img/toy-project-where-is-the-bus-1.png){: target="_blank" }

### 실시간 노선 번호 검색
1. 공공 API > 버스노선 조회 서비스 > 노선 번호 목록 조회를 이용했다.
2. 키보드 누를 때마다 keyup 이벤트가 발생하여, 입력된 숫자로 시작하는 노선 목록을 출력해준다. 글자 수+1 길이의 노선까지 제공한다.
    * 예: 한 자리 수 ‘3’ 입력 시, 최대 두 자리 수 번호까지 제공 ‘3’, ’30’, ’31’ …
3. 목록에서 노선을 선택하면, 인풋에 해당 내용이 반영되고 hidden 인풋(routeId, routeName)에도 값을 저장한다.
4. 노선 번호 수정을 위해 인풋을 클릭하면, 지역 및 버스 유형 정보가 지워지고 노선 번호만 남게 했다.
    * hidden 인풋 routeName의 value를 가져온다.
    * 수정에 용이하면서, 백스페이스 키를 누르면서 발생하는 keyup 이벤트를 줄이기 위함이다.
5. 노선 목록이 4개 이상일 경우 스크롤 바를 제공하고, 3개 이하일 땐 개수만큼 select 태그의 높이를 조정했다.

[<img src="{{site.baseurl}}/assets/post_img/toy-project-where-is-the-bus-2.gif">]({{site.baseurl}}/assets/post_img/toy-project-where-is-the-bus-2.gif){: target="_blank" }

### 주변 정류장에서 선택된 노선의 도착 정보 제공
1. 공공 API > 정류소 조회 서비스 > 주변 정류소 목록 조회, 공공 API > 버스 도착 정보 조회 서비스 > 버스 도착 정보 목록 조회를 이용했다.
2. 좌표 값 기준으로 주변 정류소 조회 후, 각 정류소에 대해 선택된 노선(routeId 비교)의 도착 정보를 조회하여 제공하는 방식이다.
3. 조회된 도착 정보는 res.render를 통해 pug 템플릿에 전달하고, pug 템플릿에서 데이터를 순회하며 리스트(li 태그)를 생성하도록 만들었다.

[<img src="{{site.baseurl}}/assets/post_img/toy-project-where-is-the-bus-3.png">]({{site.baseurl}}/assets/post_img/toy-project-where-is-the-bus-3.png){: target="_blank" }

### 오류 페이지 제공
1. 주변에 정류장이 없을 경우 또는 주변 정류장에 선택한 노선이 운행하지 않을 경우 오류 페이지가 제공된다.
2. catch를 통해 오류 케이스를 인식하고, 오류 메세지를 pug 템플릿에 전달했다.

[<img src="{{site.baseurl}}/assets/post_img/toy-project-where-is-the-bus-4.png">]({{site.baseurl}}/assets/post_img/toy-project-where-is-the-bus-4.png){: target="_blank" }

### 맺음말
아직 도착정보 조회 페이지는 디자인도 못 입힌 상태고, 여러모로 더 디벨롭해야할 부분이 많다.   
특히 노선 번호 선택 안 했을 때 폼 제출 막고 얼럿 메세지 띄우는 거 꼭 해야겠다. 만들다 보니 기획도 꼭 변경해야될 게 보여서 제대로 기획서도 만들 예정이다. 0.5 버전 목표로 구현되어야할 것, 1.0 버전으로 구현되어야 할 것으로 나누어서 작성해야겠다.   
해야할 것도 많고 새벽까지 디버깅을 하지만 너무너무 재미있는 프로젝트다. 꼭 완성까지 달릴 수 있길 🤩