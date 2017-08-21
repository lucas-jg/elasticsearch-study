## Logstash
Logstash는 기본적으로 다양한 소스를 효율적으로 수집을 할 수 있게 도와주는 오픈소스입니다.
입력, 필터, 출력 3가지 단계가 있으며 각 단계별로 다양한 플러그인이 있어 다양한 서비스 사이에서 사용될 수 있죠. 

### Logstash 다운로드
최신 버전 다운로드 : 
[https://www.elastic.co/kr/downloads/logstash](https://www.elastic.co/kr/downloads/logstash)

지난 버전 다운로드 (Past Releases) : [https://www.elastic.co/kr/downloads/past-releases](https://www.elastic.co/kr/downloads/past-releases)

링크로 이동하면 logstash를 받을 수 있으며 저는 logstash-5.5.0 버전을 사용하여 글을 작성중입니다.

아래 설치 및 실행관련 부분은 리눅스 환경을 기준으로 적었으며, 윈도우 환경에서도 동일하게 진행하면 문제 없이 사용가능합니다. 

### Getting Started with Logstash
logstash 설치는 정말 간답합니다. 위에서 다운로드 받은 tar파일을 원하는 서버에 다운받아 압축을 풀면 모든 준비는 끝났습니다.

* 다운로드 및 설치 요약 
```
# wegt 'https://artifacts.elastic.co/downloads/logstash/logstash-5.5.0.tar.gz'

# tar -xvf ./logstash-5.5.0.tar.gz
# cd ./logstash-5.5.0
# ls -al
합계 228
drwxr-xr-x. 12 root root   4096 2017-07-12 09:30 .
dr-xr-x---. 35 root root   4096 2017-08-17 09:50 ..
-rw-r--r--.  1 root root 111573 2017-07-01 08:51 CHANGELOG.md
-rw-r--r--.  1 root root   2249 2017-07-01 08:51 CONTRIBUTORS
-rw-r--r--.  1 root root   3874 2017-07-12 11:14 Gemfile
-rw-r--r--.  1 root root  21427 2017-07-01 08:52 Gemfile.jruby-1.9.lock
-rw-r--r--.  1 root root    589 2017-07-01 08:51 LICENSE
-rw-r--r--.  1 root root  29345 2017-07-01 08:55 NOTICE.TXT
drwxr-xr-x.  2 root root   4096 2017-08-11 16:31 bin
drwxr-xr-x.  2 root root   4096 2017-07-11 19:01 config
drwxr-xr-x.  5 root root   4096 2017-07-12 09:30 data
drwxr-xr-x.  5 root root   4096 2017-07-11 19:01 lib
drwxr-xr-x.  2 root root   4096 2017-08-16 11:13 logs
drwxr-xr-x.  4 root root   4096 2017-07-12 11:14 logstash-core
drwxr-xr-x.  3 root root   4096 2017-07-11 19:01 logstash-core-plugin-api
drwxr-xr-x.  3 root root   4096 2017-07-11 19:01 modules
drwxr-xr-x.  3 root root   4096 2017-07-11 19:01 tools
drwxr-xr-x.  4 root root   4096 2017-07-11 19:01 vendor
```

이걸로 logstash 설치는 과정은 끝이나지만, logstash를 제대로 사용하기 위해서는 플러그인 필요합니다. 


### Logstash install plugin
이글을 시작하면서 언급했던것 같이 logstash의 기능중 가장 매력있는것은 바로 각 기능별로 지원하는 다양한 플러그인 입니다. 

**플러그인 목록**

Input : [https://www.elastic.co/guide/en/logstash/current/input-plugins.html](https://www.elastic.co/guide/en/logstash/current/input-plugins.html)

Filter : [https://www.elastic.co/guide/en/logstash/current/filter-plugins.html](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html)

Output : [https://www.elastic.co/guide/en/logstash/current/output-plugins.html](https://www.elastic.co/guide/en/logstash/current/output-plugins.html)

**설치된 플러그인 확인**
```
bin/logstash-plugin list 
bin/logstash-plugin list --verbose 
bin/logstash-plugin list '*namefragment*' 
bin/logstash-plugin list --group output 
```

플러그인 설치
```
bin/logstash-plugin install logstash-input-file
bin/logstash-plugin install /path/to/logstash-output-kafka-1.0.0.gem
```

플러그인 업데이트
```
bin/logstash-plugin update 
bin/logstash-plugin update logstash-output-kafka 
```

플러그인에 대한 자세한 내용은 [Elastic logstash document](https://www.elastic.co/guide/en/logstash/current/working-with-plugins.html#listing-plugins) 에서 확인 가능 합니다.


다음 글에서는 다운 받은 플러그인을 사용하기 위한 설정 및 logstash 실행관련 내용을 정리하도록 하겠습니다.

## 참고 자료
Elastic logstash document

[https://www.elastic.co/guide/en/logstash/current](https://www.elastic.co/guide/en/logstash/current)


