## 논리적 레이아웃

Elasticsearch(이하 ES)의 논리적 레이아웃에는 index, type, document가 있으며, 색인과 검색을 위한 데이터의 가장 작은 단위는 document 이다.

우선 논리적 레이아웃 각각의 개념 정리와 추가적으로 알게된 내용 정리

### 인덱스/타입/문서(index/type/document)
ES의 논리적 레이아웃이다. 데이터 계층이라고도 말할 수 있다.
기존에 익숙한 RDBMS와 비교하여 설명하면 조금더 이해하기 쉬울것 같아 내용을 정리한다면

| ElasticSearch |   RDBMS    |
|---------------|:----------:|
| Index         | Database   |
| Type          | Table      |
| Document      | Rows       |
| Column        | Field      |
| Schema        | Mapping    |
| SQL           | Query DSL  |


또한 ES는 RESTful API를 통해 질의를 할 수 있으며 표현은 아래와 같이 한다.

**http://{Elasticsearch-host}:{port}/index/type/ID(document)**

### Index

### Type

### Document

## 참고 자료
Elasticsearch in Action - understanding the logical layout documents, type, index

[https://weng.gitbooks.io/elasticsearch-in-action](https://weng.gitbooks.io/elasticsearch-in-action/content/chapter2_diving_into_the_functionality/21understanding_the_logical_layout_documents_,type.html)

jjeong blog - Elastic

[http://jjeong.tistory.com/](http://jjeong.tistory.com/)

박연오 blog

[https://bakyeono.net/](https://bakyeono.net/)