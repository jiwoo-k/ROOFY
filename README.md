# 🚀 프로젝트 명: ROOFY (위치 기반 커뮤니티)

<p>
"한 지붕(지역) 아래 사람을 연결한다"
</p>

---

## 📖 프로젝트 소개
커뮤니티 활동을 하다 보면 다양한 게시판, 수많은 게시글, 사용자들 속에서 길을 잃기 쉽습니다. 따라서 사용자들이 편리하게 사용하면서 건강한 소통을 할 수 있는 커뮤니티 서비스를 만들고자 하였습니다.

## 📅 개발 기간
* 2025.05.01 ~ 2025.06.05

## 🧑‍🤝‍🧑 멤버
* **김지우(팀장)**
  * Spring Security를 이용한 OAuth2 기반 로그인/로그아웃 및 회원가입 구현
  * Geocoder/Geolocation/Google Map API 를 활용한 사용자 위치 설정
  * 사용자 위치 기반 거리별 게시물 조회
  * EC2 및 RDS 를 활용한 AWS 배포
  * 관리자 페이지 - 사용자 신고 상세 내역 및 이에 따른 계정 상태 관리
  * 해시태그 기반 검색 및 태그 생성이 가능한 템플릿 구현
  * Github 세팅 및 관리

* **김충희** 
  * 게시판 유형에 따른 게시판 및 게시물의 CRUD 구현
  * 사용자 간 신고 및 팔로우 관계 기능 구현
  * 태그 템플릿과 게시판 CRUD 연동

* **김현구**
  * ThymeLeaf Security 를 이용한 헤더 구현
  * AJAX 기반 댓글/대댓글 CRUD 및 댓글 PICK 기능 구현
  * 마이페이지(내가 작성한 게시물, 댓글, PICK한 댓글, 팔로잉 목록 조회 & 회원정보 수정) 구현

* **손범규**
  * pagination 구현
  * 게시물 첨부파일 CRUD 구현
  * 관리자페이지 - 카테고리 관리
  * 전체 CSS

## 🛠️ 기술 스택 (Tech Stack)

### Backend
<li>
IDE: IntelliJ
</li>
<li>
Java 17, Spring Boot 3.4.4, Spring security, Spring Web, Mybatis, gradle, Thymeleaf
</li>

### Frontend
<li>HTML5, CSS3, bootstrap 5.3.0, jQuery 3.7.0, ajax, chart.js</li>

### Database & Cloud
<li>MySQL, AWS EC2 & RDS</li>

### Location & Map API
<li>Google Maps, Geolocation, Geocoder API</li>

### Tools & Etc
<li>PostMan, DrawIO</li>

## 🔧 아키텍처 (Architecture)

### DB 구조 (ERD)
![erd.png](erd.png)

### 화면 흐름도
![page_flow.png](page_flow.png)

## 🔧 트러블슈팅 (TroubleShooting)

### 🧐 문제점 (Problem)

> **문제 상황:** 위치 기반 서비스의 핵심인 사용자 위치 데이터가 프론트엔드에만 머물러 거리 계산 등 핵심 로직을 수행해야 할 백엔드에서 해당 정보를 활용할 수 없는 구조적 문제가 발생했습니다.

* **원인 분석:** 초기 설계 단계에서 UI 구현에만 집중한 나머지, 데이터의 흐름을 시스템 전체 관점에서 고려하지 못했습니다.

### 🛠️ 해결 과정 (Solution)

> **해결 전략:** 서비스의 핵심 데이터인 '사용자 위치'를 **서버가 직접 관리**하도록 했습니다.

1.  **[Front-End] 비동기 통신으로 위치 정보 서버 전송**
    * JavaScript의 `fetch` API를 사용하여 사용자의 위도, 경도 정보를 서버의 `/location` 엔드포인트로 비동기 전송하는 로직을 추가했습니다.
    ```javascript
    // 사용자의 현재 위치를 얻어 서버로 전송하는 로직(예시)
    navigator.geolocation.getCurrentPosition(position => {
      const { latitude, longitude } = position.coords;
      fetch('/location', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ latitude, longitude })
      });
    });
    ```

2.  **[Back-End] 위치 정보를 수신하는 API 엔드포인트 구현**
    * Spring MVC의 `@PostMapping`을 사용하여 `/location` 요청을 처리하고 클라이언트로부터 전달받은 위치 데이터를 DTO로 매핑하여 수신했습니다.

3.  **[Back-End] 사용자 상태에 따른 데이터 관리 로직 구현**
    * **로그인 사용자:** 서비스의 핵심 기능이므로 전달받은 최신 위치를 즉시 DB에 업데이트하여 영속적으로 관리했습니다.
    * **비로그인 사용자:** 비로그인 상태에서도 위치 기반 서비스를 경험할 수 있도록, 세션(Session)에 위치 정보를 임시 저장하는 이원화된 처리 방식을 구현했습니다.

### 📚 배운 점

* **시스템 전체를 보는 아키텍처 설계의 중요성:** 기능 개발 시 눈에 보이는 UI뿐만 아니라, **데이터가 어디서 생성되고 어떻게 흘러가는지**를 시스템 전체 관점에서 설계해야 한다는 것을 깨달았습니다.
* **프론트엔드와 백엔드의 명확한 역할 분리:** 서비스의 핵심 로직과 데이터는 **서버 측에서 관리**해야 한다는 것을 배웠습니다.

## 🔗 링크
* **시연 영상:** [YouTube 링크](https://www.youtube.com/watch?v=S6Q-LdETwic&list=PLedGoSru794_lz0PHBO9FshQEC-O5IpYm&index=3)
