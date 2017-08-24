## Mappings

Elasticsearch에서 사용하는 Mapping를 대해 알아보도록 하겠습니다.
가장 기본적인 내용이면서 가장 중요한 내용입니다. 초기 Index 생성시 Mappings을 각 Field 용도에 맞춰 설계하는 것이 가장 중요합니다.

Index 생성 시 Mappings를 설정하고 데이터를 색인하게 되면 그 이후로 Mppaings를 변경할 수 없습니다. 단, 추가는 언제든지 가능합니다.

그렇다고 절대로 Mapping를 변경할 수 없는 것은 아닙니다. Alias를 이용하여 백그라운드에서 데이터를 재인덱싱하여 변경하는 방법은 있지만, 이 내용은 여기서 다루지 않겠습니다.

자세한 내용이 궁금하신 분들은 아래 링크를 통해 확인하세요.

[https://www.elastic.co/guide/en/elasticsearch/reference/5.4/mapping.html](https://www.elastic.co/guide/en/elasticsearch/reference/5.4/mapping.html)



기본적으로 Mapping를 설정하지 않아도 Elasticsearch에서 자동으로 설정을 해주지만 조금 더 효율적으로 사용하기 위해 자신이 원하는 Mapping를 설정하는 것이 가장 좋은 방법입니다. 

현재 글을 쓰는 시점에는 Elasticsearch 5.5.2 버전이 릴리즈 된 상태입니다. Google에 Mapping 관련 내용 검색 시 대부분의 글들은 2.x 버전 기준의 글이 많아서 5.x 버전 기준으로 작성 후 2.x버전에서 변경된 내용도 함께 정리하겠습니다.

---

### Elasticsearch Document - Mapping 의 기본 분류
1. Field datatypes
2. Meta-Fields
3. Mapping parameters
4. Dynamic Mapping

### Field datatypes
1. string
    * text and keyword
2. Numeric datatypes
    * long, integer, short, byte, double, float, half_float, scaled_float
3. Date datatype
    * date
4. Boolean datatype
    * boolean
5. Binary datatype
    * binary
6. Range datatypes
    * integer_range, float_range, long_range, double_range, date_range

String datatypes 에는 text, keyword 두가지 타입이 있습니다. 이부분은 5.x 버전에서 새롭게 추가된 내용이며, 기존 2.x 버전에서는 stirng 타입으로 동일하게 사용했습니다.

text와 keyword은 둘다 string을 위한 field datatype 입니다. 그렇다면 뭐가 다를까요?

text는 full-text index(전문 검색/색인)를 위한 타입이며, keyword는 일반 index(단순 검색)을 위한 타입니다.

"elasticsearch"를 색인한다고 가정하겠습니다. text를 사용한다면 저장되는 terms는 elastic, elasticsearch, search 입니다. 
keyword를 사용한다면 저장되는 terms는 elasticsearch가 됩니다.

간단하게 설정하는 방법을 확인 해보겠습니다.
```
{
    "text_field": {
        "type": "text",
        "index": true
    },
    "keyword_field": {
        "type": "keyword",
        "index": true
    },
    "keyword_field_not_search": {
        "type": "keyword",
        "index": false
    }
}
```

여기서  "index": true 의 경우 default이기 때문에 생략이 가능합니다.
"index" : flase로 설정하는 경우 검색조차 되지 않기 때문에 필요에 따라 설정합니다.

이번에는 2.x 버전에서 사용했던 방법으로 알아보겠습니다. 먼저 위에서 사용한 설정 방법과 동일한 내용입니다.

```
{
    "text_field": {
        "type": "stirng",
        "index": "analyzed"
    },
    "keyword_field": {
        "type": "stirng",
        "index": "not_analyzed"
    },
    "keyword_field_not_search": {
        "type": "stirng"",
        "index": "no"
    }
}
```

### 2.x와 5.x 차이점 정리

| Version | Datatype |  Inedx         | Text(Stinrg)  | Terms                          |
|---------|----------|:--------------:|:-------------:|:------------------------------:|
| 2.x     | string   | "analyzed"     | elasticsearch | elastic, search, elasticsearch |
| 2.x     | string   | "not_analyzed" | elasticsearch | elasticsearch                  |
| 2.x     | string   | "no"           | elasticsearch | -                              |
| 5.x     | text     | true           | elasticsearch | elastic, search, elasticsearch |
| 5.x     | keyword  | true           | elasticsearch | elasticsearch                  |
| 5.x     | keyword  | false          | elasticsearch | -                              |

### 새로운 Index 생성 및 Mapping 설정

Elasticsearch 5.x 버전
```
{
	"settings": {
		"index": {
			"number_of_shards": 10,
			"number_of_replicas": 1,
			"refresh_interval": "10s"
		}
	},
	"mappings": {
		"my_type": {
			"properties": {
				"text_field": {
					"type": "text",
					"index": true
				},
				"keyword_field": {
					"type": "keyword",
					"index": true
				},
				"integer_field": {
					"type": "integer",
					"index": true,
					"ignore_malformed": true
				},
				"ip_field": {
					"type": "ip",
					"index": true
				},
				"integer_field_index_false": {
					"type": "integer",
					"index": false,
					"ignore_malformed": true
				},
				"keyword_field_index_false": {
					"type": "keyword",
					"index": false
				},
				"object_field": {
					"properties": {
						"text-field": {
							"type": "text",
							"index": true
						},
						"keyword_field": {
							"type": "keyword",
							"index": true
						},
						"integer_field": {
							"type": "integer",
							"index": true,
							"ignore_malformed": true
						}
					}
				}
			}
		}
	}
}
```

Elasticsearch 2.x 버전
```
{
	"settings": {
		"index": {
			"number_of_shards": 10,
			"number_of_replicas": 1,
			"refresh_interval": "10s"
		}
	},
	"mappings": {
		"my_type": {
			"properties": {
				"text_field": {
					"type": "string",
					"index": "analyzed"
				},
				"keyword_field": {
					"type": "string",
					"index": "not_analyzed"
				},
				"integer_field": {
					"type": "integer",
					"index": "not_analyzed",
					"ignore_malformed": true
				},
				"ip_field": {
					"type": "ip",
					"index": "not_analyzed"
				},
				"integer_field_index_false": {
					"type": "integer",
					"index": false,
					"ignore_malformed": true
				},
				"keyword_field_index_false": {
					"type": "string",
					"index": false
				},
				"object_field": {
					"properties": {
						"text-field": {
							"type": "string",
							"index": "analyzed"
						},
						"keyword_field": {
							"type": "string",
							"index": "not_analyzed"
						},
						"integer_field": {
							"type": "integer",
							"index": "not_analyzed",
							"ignore_malformed": true
						}
					}
				}
			}
		}
	}
}
```

P.S - refresh_interval 설정은 색인하는 주기를 설정하는 값입니다. 검색 관련 성능 향상을 위해 변경이 가능하며 대부분의 길에 성능을 위해 설정하는 경우 30s으로 설정하는 경우를 많이 보았습니다. 각자 운영하는 elasticsearch 상황에 따라서 설정하면 될 것 같습니다. 위 내용은 다음에 기회가 되는 경우 자세하게 포스팅하겠습니다.

짧게 상대적으로 많이 사용될 것으로 예상되는 기초적인 내용 위주로 정리하였습니다. 더 많은 정보를 원하는 경우 ElasticSearch 공식 Document를 확인해주세요.

[https://www.elastic.co/guide/en/elasticsearch/reference/5.5/mapping.html](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/mapping.html)


---

## 참고 자료

ElasticSearch Document - Mapping

[https://www.elastic.co/guide/en/elasticsearch/reference/5.5/mapping.html](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/mapping.html)

jjeong blog - Elastic

[http://jjeong.tistory.com/](http://jjeong.tistory.com/)
