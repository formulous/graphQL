# graphQL

# 개요
* GraphQL 기술 조사 및 세미나

---------------------------------------------------------------------------------------------

## GraphQL?
* API를 더욱 빠르고 유연하며 개발자 친화적으로 만들기 위해 설계된 쿼리 언어입니다.

* 클라이언트에게 요청한 만큼의 데이터를 제공하는 데 우선 순위를 둡니다.
 
* SQL이 DataBase에 저장된 데이터를 효율적으로 가져오는 것이 목적이라면, GQL(GraphQL)은 웹클라이언트가 데이터를 서버로 부터 효율적으로 가져오는 것이 목적입니다.

* SQL은 주로 백앤드 시스템에서 작성하고 호출하는 반면, GQL은 주로 클라이언트 시스템에서 작성하고 호출하게 됩니다.

* 클라이언트가 자신에게 필요한 query를 선언해 graphQL에 넘기면, GraphQL은 해당 query를 해석해서 서버에서 필요한 데이터를 가져와 클라이언트에 데이터 반환합니다.

![image](https://user-images.githubusercontent.com/88424067/191893040-5d630f27-dc3a-448a-bd64-54ccb0960c53.png)

## GraphQL 쿼리 예시
|특정 필드에 대한 요청|요청 결과|
|---|---|
|<pre>{<br>  hero {<br>    name<br>    # 쿼리에 주석을 쓸 수도 있습니다!<br>    friends {<br>      name<br>    }<br>  }<br>}</pre> | <pre>{<br>  "data": {<br>    "hero": {<br>      "name": "R2-D2",<br>      "friends": [<br>        {<br>          "name": "Luke Skywalker" <br>        },<br>        {<br>          "name": "Han Solo" <br>        },<br>        {<br>          "name": "Leia Organa" <br>        }<br>      ]<br>    }<br>  }<br>}</pre>|
## GraphQL 파이프라인

![image](https://user-images.githubusercontent.com/88424067/191893004-dc903ff8-bbe7-42cf-ae70-ce7bd9b85300.png)

----------------------------------------------------------------------------------------------

## REST API와 비교

|REST API | GraphQL |
|URL, METHOD등을 조합하여 다양한 Endpoint가 존재합니다. (여러 Resource에 접근할 때 여러번의 요청이 필요) |단 하나의 Endpoint가 존재 합니다. (한번의 요청으로 다양한 Resource에 접근 가능)|
|Endpoint마다 데이터베이스 sql쿼리가 달라집니다.|gql 스키마의 타입마다 데이터베이스 sql쿼리가 달라집니다.|
|Resource의 크기와 형태를 서버에서 결정합니다.|Resource에 대한 정보만 정의하고, 필요한 크기와 형태는 client단에서 요청 시 결정합니다.|
|URI가 Resource를 나타내고 Method가 작업의 유형을 나타냅니다.|GraphQL Schema가 Resource를 나타내고 Query, Mutation 타입이 작업의 유형을 나타냅니다.|
|각 요청은 해당 엔드포인트에 정의된 핸들링 함수를 호출하여 작업합니다.|요청 받은 각 필드에 대한 resolver를 호출하여 작업을 처리합니다.|
|*Rest API 네트워크 호출 과정
(REST API 네트워크 호출과정 IMAGE)
|GraphQL 네트워크 호출 과정
(GraphQL 네트워크 호출 과정 IMAGE)
|
|Rest API 요청과 응답
<pre>
[GET] /books/1

{
  "title": "Romance of the Three Kingdoms",
  "author": {
    "firstName": "Luo",
    "lastName": "Guanzhong"
  }
}<pre?
|
GraphQL 요청과 응답
<pre>
// Type 지정 (형태만 지정한 상태)
type Book {
  id: ID
  title: String
  author: Author
}
type Author {
  id: ID
  firstName: String
  lastName: String
  books: [Book]
}

// 지정한 형태에 접근할 수 있도록 query 타입이 필요 
type Query {
  book(id: ID!): Book
  author(id: ID!): Author
}

// 요청 방법
/graphql?query={ book(id: "1") { title, author { firstName } } }

// 응답 데이터
{
  "title": "Black Hole Blues",
  "author": {
    "firstName": "Janna"
  }
}
</code></pre>
}}
|
|*일반 HTTP API 적용 스택*
 (일반 HTTP API 적용 스택 이미지)
| *GraphQL 적용 스택*
( GraphQL 적용 스택 이미지)
|
|REST API 동작 예시
(restapi gif)
|GraphQL 동작 예시
(graphQL gif)|

## 스키마/타입(schema/type)

* 스키마란 데이터 타입의 집합으로 클라이언트, 서버 개발자같의 의사소통 비용을 줄이고 빠르게 개발할 수 있게 돕는 API 문서 역할을 담당합니다.
* GraphQL의 query  형태는 리턴되는 값과 거의 일치하기 때문에 API 설계 전 사용할 스키마를 정의해야 합니다.
* 타입 (Type)
** 스키마의 핵심 단위로, 커스텀 객체로 작성하여 이를 통해 애플리케이션의 핵심 기능을 알 수 있습니다.
<pre><code class="javascript">
type Character {
  name: String!
  appearsIn: [Episode!]!
}

// 오브젝트 타입 : Character
// 필드 : name, appearsIn
// 스칼라 타입 : String, ID, Int 등
// 느낌표(!) : 필수 값을 의미(non-nullable)
// 대괄호([, ]) : 배열을 의미(array)
</code></pre> 
** 스키마 대부분의 타입은 위와 같은 일반 객체 타입이지만 특수한 2가지 타입인 qeury, mutation 타입이 존재합니다.
