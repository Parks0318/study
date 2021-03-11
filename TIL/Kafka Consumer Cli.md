## 카프카 컨슈머 offset을 되돌리는 법

개발을 진행하며 테스트 시 반복해서 컨슘해야하는 경우 offset을 되돌릴 필요가 있었다


* 컨슈머의 상태를 확인하는 명령어 --describe 사용하여 현황 확인
```
kafka-consumer-groups --bootstrap-server {host}:{port} --group {group.id} --describe

아래를 중점적으로 볼 필요가 있음
* CURRENT-OFFSET: 마지막으로 offset을 commit한 값
* LOG-END-OFFSET: producer쪽에서 마지막으로 생성한 레코드의 offset
* LAG: LOG-END-OFFSET에서 CURRENT-OFFSET를 뺀 값
```

* 컨슈머 오프셋을 원하는 부분으로 설정 --reset-offsets --to-offset--reset-offsets --to-offset 
```
kafka-consumer-groups --bootstrap-server {host}:{port} --group {group.id} --topic {topic.name} --reset-offsets --to-offset {offset.value} 
```
