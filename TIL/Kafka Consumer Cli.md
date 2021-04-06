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

* 컨슈머를 수동으로 종료하는 방법 (code)
```
블루그린 배포 과정에서 인스턴스 종료 전 수동으로 컨슈밍 종료가 필요한 경우가 있었다.
-> 만약 종료하지않고 인스턴스를 내릴 경우 메세지 유실 가능성이 있음

전제 : Kafka를 조정할 Controller 생성 후 MessageListenerContainer를 이용하여 stop 


1. 컨슈머 그룹 확인 후 종료가 필요한 HOST확인 --describe
2. 종료 대상인 HOST에 접속
3. curl -X GET "http://localhost:5000/kafka/stop?id={group.id}" -H "accept: */*"
```