### 240225
## [High Quality Video Encoding at Scale](https://netflixtechblog.com/high-quality-video-encoding-at-scale-d159db052746)
### 데이터 프로세싱 방법에 대해 궁금해서 찾아보았다.
### 이런 블로그 글들을 보면 항상 공통적인 것들이 있다. 거의 모든 건 결국 속도로 종합할 수 있다. 
- 제한된 resource를 어떻게하면 효율적이고, 전체적으로 사용할 수 있을까
- 병렬 처리
- 큰 데이터는 어떻게 하면 빨리 처리할 수 있을까 : autoscaling, elasticity of the cloud
- 데이터 표준(standard) 정하기 -> 그리고 표준을 정함으로써 데이터를 나눌 수 있고, 정해진 포맷에 따라 나눈 걸 다시 합칠 수도 있음
### <br/><br/><br/>

## overview
### 이 블로그 컨텐츠에서 설명하는 overview이다. 
### 비디오들을 각각 인식 -> ingest -> processing -> packaging -> deployment 순서다.
### processing에는 다양한 codec file의 encoding 방식을 지원하는 것, 병렬 처리 등이 있다.
### deploy는 content delivery network for streaming에 따라 속도가 달라진다.
#### ![image](https://github.com/Shin-jongwhan/tech_blog/assets/62974484/f8eb5c68-cd46-4f1a-843c-7cd059af61e4)
