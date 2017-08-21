## 물리적 레이아웃

엘라스틱서치의 물리적 레이아웃에 해당하는 샤드(Shard)와 노드(node), 클러스터(Cluster)를 반드시 이해하고 넘어 가야한다. 이유는 실제 엘라스틱서치를 이용하여 빅데이터 처리, 검색 시스템을 운영하면서 엘라스틱서치 서버 확장이 필요한 시기가 올 수 있기 때문이다.


### 클러스터 (Cluster)
클러스터는 노드의 모임, 엘라스틱서치의 모임이라는 생각일 가지고 접근한다면 좀 더 이해하기 쉬운 것 같다. 각 노드는 데이터를 나눠 저장하고 있으며 해당 노드들을 하나로 묶어 주는 개념이다.

클러스터에 노드를 추가하는 방법으로 확장하는 것을 수평적 확장(Horizontal scaling)이라고 부르며 이 경우 각 노드는 각자의 역할을 수행하고 모든 노드가 일을 공유하여 처리하기 때문에 부가적인 성능의 이득을 얻는다.

### 노드 (Node)
간단히 말하면 노드는 엘라스틱서치를 실행하는 프로세스라고 말할 수 있으며, n개의 샤드로 구성된다. 하나의 서버 내부에서 엘라스틱서치를 여러 번 실행하면 하나의 서버 안에 n개의 노드를 가질 수 있다. 만약 또 다른 서버에 엘라스틱서치를 실행했다면 그 또한 노드가 생성되는 것이고 n개의 노드들을 하나로 묶기 위해서는 클러스터(Cluster)를 이용한다.

클러스터로 묶인 노드들은 각자의 역할을 수행을 할 수 있다. 각 노드에 역할을 주기 위해서는 config/elasticsearch.yml 에서 간단하게 수정이 가능하다.

#### 노드의 역할
1. 데이터 노드 (Data node) - node.data : true 
    * 데이터가 저장되는 노드, 색인 및 검색 작업이 수행되는 노드이다.
2. 마스터 노드 (Master node) - node.master : true
    * 클러스터를 관리하는 노드, 큰 구성에서는 n개의 마스터 노드를 생성하여 관리한다.
1. 클라이언트 노드 (Client node) - http.enabled : true
    * 네트워크 부하를 담당하며 각 노드의 앞단에서 로드 밸런서의 역할을 수행한다.

노드의 역할에 따라 옵션을 변경해주면 되고 하나의 노드가 3가지 역할을 모두 수행할 수도 있다. 단일 노드 구성의 경우 그만큼 위험부담이 따른다.

`Elasticsearch에서 색인 및 검색을 위해서 대부분 Elastic에서 제공하는 Java API와 RESTful API를 사용한다. 이때 주의할 점은 Java API는 Transport 모듈로 통신을 하기 때문에 데이터 노드와 통신을 하며, RESTful API는 HTTP 모듈로 통신하기 때문에 클라이언트 노드와 통신을 한다.`

### 샤드(Shard) / 리플리카(Replica)

하나의 샤드는 하나의 루씬 색인이다. 루씬 색인이란 역 색인을 포함한 파일들의 모음이라고 할 수있다. 리플리카는 샤드의 복제본이다. 

엘라스틱서치에서 색인을 생성하는 경우 기본 설정으로 샤드 5개와 샤드당 리플리카 1개가 생성 된다. 즉 1개의 색인을 생성하면 10개의 샤드가 생성되는 것이다.

색인을 생성하는 시점에서 샤드 수를 지정할 수 있다. 하지만 색인에 데이터가 색인된 후에는 샤드의 수를 수정할 수 없다. 그에 반해 리플리카의 추가/제거는 항상 가능하다.







## 참고 자료
Elasticsearch in Action - understanding the logical layout documents, type, index

[https://weng.gitbooks.io/elasticsearch-in-action](https://weng.gitbooks.io/elasticsearch-in-action/content/chapter2_diving_into_the_functionality/21understanding_the_logical_layout_documents_,type.html)

behonestar - Tistory

[http://behonestar.tistory.com](http://behonestar.tistory.com)