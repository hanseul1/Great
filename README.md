# Sub-PJT 03

## 기본환경세팅

### Frontend

- `Node.js`: v12.14.0
- `npm`: 6.13.4
- `vue`: 2.6.11
  - `vuecli` : `npm install -g @vue/cli` : 4.1.2
- package.json dependencies
```
"dependencies": {
  "core-js": "^3.4.4",
  "vue": "^2.6.10",
  "vue-router": "^3.1.3",
  "vuetify": "^2.1.0",
  "vuex": "^3.1.2"
  }
```

### Backend

- `Spring boot`
  - `STS(Spring-Tool-Suite)` : 2.2.3 RELEASE
  - `Java` : v1.8
  - `maven-jar-plugin` : 3.1.1
  - `myBatis` : 2.1.1
- `AWS`
  - `Docker` : 19.03.5 ver
  - `MySQL` : 8.0.19

## 디렉토리 구조

```
.
|-- README.md
|-- back
|   |-- Dockerfile.txt
|   |-- mvnw
|   |-- mvnw.cmd
|   |-- pom.xml
|   |-- sql
|   |-- target
|   `-- src
|       `--main
|           |-- java
|           |   `-- com
|           |       `-- ssafy
|           |           `-- great
|           |               |-- GreatApplication.java
|           |               |-- config
|           |               |-- controller
|           |               |-- dto
|           |               |-- filter
|           |               |-- interceptor
|           |               |-- model
|           |               `-- util
|           `-- resources
|               |-- application.properties
|               |-- config
|               |   |-- SqlMapConfig.xml
|               |   `-- application-config.xml
|               `-- query
|                   |-- bookmark_sql.xml
|                   |-- category_sql.xml
|                   |-- review_sql.xml
|                   |-- store_sql.xml
|                   `-- user_sql.xml
`-- front
    |-- README.md
    |-- babel.config.js
    |-- frontworkspace.code-workspace
    |-- node_modules
    |-- package-lock.json
    |-- package.json
    |-- vue.config.js
    |-- public
    `-- src
        |-- App.vue
        |-- apis
        |   `-- UserApi.js
        |-- assets
        |   `-- style
        |-- components
        |   |-- Grid
        |   |-- Tab
        |   |-- UI
        |   `-- common
        |-- main.js
        |-- plugins
        |   `-- vuetify.js
        |-- routes
        |   `-- index.js
        |-- store
        |   `-- index.js
        `-- views
            |-- Authentication.vue
            |-- Index.vue
            |-- Main.vue
            |-- Mypage.vue
            `-- PageNotFound.vue
```



## Back-End(Spring Boot)

### 데이터베이스 구조

#### 사용자 테이블 : user

| 이름      | 정의                | 타입          | 비고                        |
| --------- | ------------------- | ------------- | --------------------------- |
| id        | 사용자 구분 고유 키 | int           | auto_increment, primary key |
| email     | 이메일 주소         | varchar(30)   | sns 가입 시 null            |
| password  | 비밀번호            | varchar(20)   | sns 가입 시 null            |
| sns_token | sns 인증 토큰       | varchar(100)  | 자체 회원가입 시 null       |
| birth     | 생년월일            | Date          |                             |
| gender    | 성별                | enum("M","F") | 'M' : 남성 'F' : 여성       |
| name      | 닉네임              | varchar(10)   |                             |

#### 카테고리 테이블 : Category

| 이름 | 정의                  | 타입        | 비고                        |
| ---- | --------------------- | ----------- | --------------------------- |
| id   | 카테고리 구분 고유 키 | int         | auto_increment, primary key |
| name | 카테고리 이름         | varchar(50) |                             |

#### 식당 테이블 : store

| 이름          | 정의              | 타입         | 비고                        |
| ------------- | ----------------- | ------------ | --------------------------- |
| id            | 식당 구분 고유 키 | int          | auto_increment, primary key |
| name          | 식당 이름         | varchar(30)  |                             |
| open_time     | 영업 시간         | varchar(50)  |                             |
| map_x         | 식당 위치 x좌표   | double       |                             |
| map_y         | 식당 위치 y좌표   | double       |                             |
| location_name | 식당 위치         | varchar(50)  |                             |
| rating        | 별점              | double       |                             |
| phone         | 식당 전화번호     | varchar(15)  |                             |
| category      | 식당 카테고리     | int          | foreign key(category.id)    |
| tag           | 식당 상세 태그    | varchar(100) |                             |
| image         | 식당 사진 URL     | varchar(100) |                             |

#### 메뉴 테이블 : menu

| 이름  | 정의              | 타입        | 비고                        |
| ----- | ----------------- | ----------- | --------------------------- |
| id    | 메뉴 구분 고유 키 | int         | auto_increment, primary key |
| store | 식당 id           | integer     | foreign key(store.id)       |
| name  | 메뉴 이름         | varchar(30) |                             |
| price | 메뉴 가격         | int         |                             |

#### 리뷰 테이블 : review

| 이름     | 정의              | 타입         | 비고                        |
| -------- | ----------------- | ------------ | --------------------------- |
| id       | 리뷰 구분 고유 키 | int          | auto_increment, primary key |
| store    | 식당 id           | int          | foreign key(store.id)       |
| contents | 리뷰 내용         | text         |                             |
| date     | 리뷰 작성 날짜    | Date         |                             |
| image    | 리뷰 사진 url     | varchar(100) |                             |

#### 북마크 테이블 : bookmark

| 이름 | 정의                | 타입        | 비고                        |
| ---- | ------------------- | ----------- | --------------------------- |
| id   | 북마크 구분 고유 키 | int         | auto_increment, primary key |
| user | 사용자 id           | int         | foreign key(user.id)        |
| name | 북마크 이름         | varchar(20) |                             |
| type | 북마크 타입         | char        | 'S' : 식당 , 'G' : 그리드   |

#### 북마크 식당 테이블 : bookmarkstore

| 이름     | 정의                     | 타입 | 비고                        |
| -------- | ------------------------ | ---- | --------------------------- |
| id       | 북마크 식당 구분 고유 키 | int  | auto_increment, primary key |
| bookmark | 북마크 id                | int  | foreign key(bookmark.id)    |
| store    | 식당 id                  | int  | foreign key(store.id)       |



### REST API

#### User API

| Method | URI                 | Definition                                            |
| ------ | ------------------- | ----------------------------------------------------- |
| GET    | /user               | 모든 사용자 목록 검색                                 |
| GET    | /user/{id}          | 사용자 id에 해당하는 사용자 검색                      |
| GET    | /user/email/{email} | 사용자 email에 해당하는 사용자 검색(이메일 중복 체크) |
| POST   | /user/login         | 사용자 로그인                                         |
| POST   | /user               | 사용자 정보 등록(회원가입)                            |
| PUT    | /user               | 사용자 정보 수정                                      |
| DELETE | /user/{id}          | 사용자 id에 해당하는 사용자 정보 삭제(회원탈퇴)       |

#### Category API

| Method | URI       | Definition              |
| ------ | --------- | ----------------------- |
| GET    | /category | 모든 카테고리 목록 검색 |
| POST   | /category | 카테고리 등록           |

#### Store API

| Method | URI                                | Definition                                                   |
| ------ | ---------------------------------- | ------------------------------------------------------------ |
| GET    | /store/{id}                        | 식당 id에 해당하는 식당 정보 검색                            |
| GET    | /store/category/{category}         | 식당 category에 해당하는 식당 목록 검색<br>(별점 기준 내림차순 정렬) |
| GET    | /store/location/{category}/{x}/{y} | 식당 category에 해당하는 식당 목록 검색<br>(거리 기준 내림차순 정렬) |
| PUT    | /store                             | 식당 정보 수정                                               |
| DELETE | /store/{id}                        | 식당 id에 해당하는 식당 정보 삭제                            |

#### Review API

| Method | URI                   | Definition                                                               |
| ------ | --------------------- | ------------------------------------------------------------------------ |
| GET    | /review               | 모든 리뷰 목록 검색                                                      |
| GET    | /review/{id}          | 리뷰 id에 해당하는 리뷰 검색                                             |
| GET    | /review/store/{store} | 식당 id에 해당하는 리뷰 목록 검색<br>(리뷰 작성 날짜 기준 내림차순 정렬) |
| POST   | /review               | 리뷰 정보 등록                                                           |
| PUT    | /review               | 리뷰 정보 수정                                                           |
| DELETE | /review/{id}          | 리뷰 id에 해당하는 리뷰 정보 삭제                                        |

#### Bookmark API

| Method | URI                     | Definition                                   |
| ------ | ----------------------- | -------------------------------------------- |
| GET    | /bookmark/{user}/{type} | 사용자 id와 type에 해당하는 북마크 목록 검색 |
| GET    | /bookmark/{id}          | 북마크 id에 해당하는 식당 목록 검색          |
| POST   | /bookmark               | 북마크 정보 등록                             |
| PUT    | /bookmark               | 북마크 정보 수정                             |
| DELETE | /bookmark/{id}          | 북마크 id에 해당하는 북마크 삭제             |



#### Spatial Index

- 사용자 위치의 x좌표, y좌표 기준으로 가까운 식당 목록 20개를 데이터베이스에서 쿼리해 가져와야 한다.
- 데이터베이스의 모든 데이터와 값을 비교하고, 정렬까지 하면 DB 서버에 큰 부담이 갈 것이다.
- MySQL에서 제공하는 geospatial Index를 생성하여 빠르게 데이터를 조회할 수 있도록 구현하였다.
  - geospatial index는 내부적으로 R-Tree 구조로 데이터를 저장한다.
    - R-Tree는 2차원 이상의 공간 데이터 값을 저장하는 자료구조이다.
    - 공간 데이터값들을 최소 경계 사각형(MBR, Minimum Bounding Rectangle)으로 분할하여 인덱싱한다. (각 MBR의 경계는 서로 겹칠 수 있다.)
    - 즉, 트리의 각 노드들은 일정 범위 내의 공간 데이터를 저장하고 있어 현재 위치에서 일정 범위 내의 데이터를 조회하는데 유용하다.
    - 각 노드의 데이터가 key-value 쌍으로 이루어져 인덱싱된 B-Tree는 범위 스캔보다는 정확한 지점을 찾을 때 유리하다.
  - ST_Distance_Sphere 함수로 두 좌표 사이의 거리를 계산하여 쿼리를 수행한다.

- MySQL Query문

  ```xml
  <select id="selectByLocation" parameterType="java.util.HashMap" resultType="store">
      select *
        from store s
       where category = #{category}
         and ST_Distance_Sphere(location, point(#{x}, #{y})) <![CDATA[<]]> 10000
       order by ST_Distance_Sphere(location, point(#{x}, #{y}))
       limit 20
  </select>
  ```

  

## Front-End (Vue)

### BarButton Component

- pure HTML과 CSS만을 활용하여 직접 button vue component를 구현하였다.

- 구현 결과

  ![AppBarButton](https://user-images.githubusercontent.com/33472435/73939975-e86c7780-492d-11ea-99b1-da47a204d349.gif)

- 구현 방식

  - div 태그로 감싼 영역에 border-radius 속성으로 둥그런 버튼 모양 생성
  - 해당 div 태그에 `bar-btn` 이라는 class 이름을 부여하고, 마우스 hover시 우측 padding을 늘려주는 방식으로 transition을 주었다.
  - arrow-icon 이미지는 처음에 `display: none` css 속성을 주고, `bar-btn` 클래스에 마우스 hover시 css 속성을 `display: inline` 으로 변경하도록 하였다.
  - 마지막으로 div 태그에 v-on:click 속성 지정으로 router 이동을 구현하였다.

- 구현 코드

  ```html
  <template>
  <div>
    <div 
      class="bar-btn" 
      @click="go('Main')">
    <span>Let's Start</span>
    <img 
        src="@/assets/img/arrow-icon.png"
        class="bar-btn-img"/>
    </div>
  </div>
  </template>
  
  <script>
  import '@/assets/style/css/barButton.css'
  export default {
    name: "BarButton",
    methods: {
      go(link) {
        this.$router.push(link);
      }
    }
  };
  </script>
  ```

  ```css
  /* BarButton component */
  .bar-btn {
      font-family: "Lobster", cursive;
      background-color: #fbedeb;
      padding: 3px 20px 3px 20px;
      font-size: 1.2em;
      cursor: pointer;
      border-radius: 25px;
      display: inline;
      transition-duration: 0.5s;
    }
    .bar-btn img {
      display: none;
    }
    .bar-btn:hover {
      padding: 3px 40px 3px 20px;
    }
    .bar-btn:hover img {
      display: inline;
    }
    .bar-btn-img {
      height: 90%;
      position: absolute;
      margin-top: 2px;
    }
  ```

  

### Grid Component

- 사용자가 장소를 선택했을 때 주변 식당 목록을 카테고리별로 보여주는 메인 그리드 화면이다.
- 기본 식당을 정렬하는 기준은 사용자가 지정한 위치 기준 가까운 순서이다.

#### Data Rendering with axios & vuex

- axios

  - 사용자가 선택한 위치 기준 카테고리 목록 8개의 가까운 식당 20개 리스트를 응답해주는 REST API를 활용한다.

    - uri : /store/location/{category}/{x}/{y}

  - 문제 사항

    - 해당 API는 하나의 카테고리에 대한 식당 목록을 응답해주므로, 전체 Grid를 완성하기 위해서는 총 8번의 API 반복 호출이 필요하다.

    - 일반 for문 안에서 axios 호출과 호출 성공시 처리를 한번에 하면 axios의 비동기 수행 성질 때문에 식당 리스트의 동기화가 어려웠다.

      ```javascript
      for(var i = 0; i < categories.length; i++){
        var categoryName = categories[i].name
            // api 호출 코드
        axio.get('http://SERVER_IP:PORT/URI/variable')
            .then(response => {
            // api 호출 성공시 처리 코드
        })
      }
      ```

      => 위와 같이 코드를 구현하면, API를 호출하는 코드가 비동기적으로 먼저 수행되고, 응답이 오는 순서대로 then(...) 코드를 수행하기 때문에 categoryName을 인덱스별로 정의하는 타이밍과 동기화되지 않는다.

    - for문 안에서 각 API를 호출하는 코드를 수행하는 함수를 호출하도록 변경해 동기화 할 수 있도록 했다.

      ```javascript
      for(var i = 0; i < categories.length; i++){
        var categoryName = categories[i].name
        var categoryId = categories[i].id
        this.apiCall(categoryId, categoryName, x, y)
      }
      
      /* API 한번씩 호출하고 응답 데이터 처리하는 함수 */
      apiCall(categoryId, categoryName, x, y) {
        var data = {
          locationX : x,
          locationY : y,
          category : categoryId
        }
        GridApi.requestGridStores(data, response => {
          for(var i = response.length; i < 8; i++){
            response.push({"name": ""})
          }
          this.$store.commit(categoryName, response)
        })
      }
      ```

- Vuex

  - Vuex의 state에 각 카테고리 이름으로 식당 리스트를 저장하도록 했다.

    ```javascript
    categories: [
      {id: 1, name: "한식"},
      {id: 2, name: "중식"},
      {id: 3, name: "일식"},
      {id: 4, name: "아시아"},
      ...
    ],
    한식: [
      {id: 1, name: "한식"},
      {id: 1, name: "한식"},
      ...
    ]
    ```

    

#### Drag-and-Drop

- 식당 Grid 안의 작은 grid칸을 사용자가 drag해서 다른 영역으로 drop하면 

  해당 grid의 식당 정보가 사라지고, 새로운 식당 정보로 변경되도록 구현하였다.

- 구현 방법

  - vue의 `draggable` 속성을 true로 지정하여 해당 element를 드래그할 수 있게 하였다.

  - `v-on:dragend` 속성으로 드래그가 끝났을 때 함수를 지정하여 해당 인덱스의 식당 정보가 바뀔 수 있도록 하였다.

  - 각 카테고리별로 식당 리스트의 index 리스트를 관리하기 때문에 해당 index의 값을 빼고, 새로운 index를 넣는 방식으로 식당 정보를 바꿔주었다.

    ```javascript
    /* v-on:dragend 속성으로 지정된 함수 */
    changeIndex(i) {
      var commitName = this.categoryName + 'List'
      this.$store.commit(commitName, i)
    }
    ```

    ```javascript
    /* vuex의 카테고리별 index 리스트를 관리하는 mutations 함수 */
    '한식List'(state, payload) {
      state.한식maxIndex++;
      state.한식index.splice(payload, 1, state.한식maxIndex)
    },
    ```

- 구현 결과

  ![GridDragandDrop](https://user-images.githubusercontent.com/33472435/73988544-593f7e00-4986-11ea-9620-9961a1fd2572.gif)
  

#### mouseover & mouseleave

- 식당 Grid 안의 작은 grid 칸에 마우스 hover시 식당 이름과 별점이 함께 보이도록 구현하였다.

- 구현 방법

  - vue의 `v-on:mouseover` 속성과 `v-on:mouseleave` 속성을 지정하여 별점을 나타내는 StarRating 컴포넌트가 마우스 hover시에만 렌더링되도록 하였다.

    ```html
    <button 
       class="small-box"
       @mouseover="over(i)"
       @mouseleave="out(i)"  
       :key="idx">
      <GridItem :name="[itemName[idx].name]" />
      <!-- 별점은 mouseOn의 해당 index 값이 true일 때만 보여주도록 한다. -->
      <StarRating v-if="mouseOn[i]" :rating="itemName[idx].rating" />
      </button>
    ```

- 구현 결과

  ![GridHover](https://user-images.githubusercontent.com/33472435/73988549-5e9cc880-4986-11ea-9439-64a94cca4717.gif)
  

### StarRating Component

- Pure HTML과 CSS만 이용하여 별점을 시각화하여 나타내는 vue component를 구현하였다.

- 구현 방법

  - span 태그에 `fa-star` class를 지정하여 별을 표현하였다.

  - 채워진 별은 'checked' 라는 class 이름을 지정하여 해당 클래스에 `color: orange` css 속성을 주었다.

  - 별점의 점수는 props로 전달받아 computed로 5개의 span 태그 클래스 이름 list를 관리하도록 했다.

  - props로 전달받은 rating 값만큼 반복문을 돌아 class 이름에 'checked'를 붙여주었다.

    ```html
    <template>
      <div>
        <template v-for="(className, i) in starClass">
          <span :key="i" :class="className" class="fa-star"></span>
        </template>
      </div>
    </template>
    ```

    ```javascript
    props: ["rating"],
    computed: {
     starClass() {
      var list = []
      var className = "fa fa-star"
      for(var i = 0; i < this.rating; i++) {
        list.push(className + ' checked')
      }
      for(var j = this.rating; j < 5; j++) {
        list.push(className)
      }
      return list
     }
    }
    ```
