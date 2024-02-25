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
### 그리고 processing이 가능한 포맷인지 체크하는 단계도 포함되어 있다. 이를 inspection이라고 한다.
### deploy는 content delivery network for streaming에 따라 속도가 달라진다.
#### ![image](https://github.com/Shin-jongwhan/tech_blog/assets/62974484/f8eb5c68-cd46-4f1a-843c-7cd059af61e4)
### <br/><br/><br/>

## data processing - inspection
### inspection을 먼저 한다. processing이 가능한 포맷인지 확인한다. 15분 안쪽으로 수행된다.
#### ![image](https://github.com/Shin-jongwhan/tech_blog/assets/62974484/f559f31e-8467-4eea-932b-12e4dec8b57f)
### <br/><br/><br/>

## parallel video encoding
### 클라우드의 발전으로, 다양한 크기의 소스를 병렬처리하는 것이 가능해졌다.
### inspection이 끝나면 chunk 사이즈로 나눠서 프로세싱을 진행한다.
### 프로세싱이 끝나고나면 각 chunk를 합치는 assembly 작업을 진행한다.
### 이 때 data validation 작업이 진행되는데, order가 잘못되었거나 duplicated chunk가 있다거나, frame drop이 발견되었거나 하는 등을 검사한다.
### <br/>

### 설명은 잘 안 해주는데, 어떤 chunk에서 에러가 났을 때 전체가 프로세싱이 안 되서 이 부분에 대해서 굉장히 어려움을 겪고 있다고 한다.
### 그래서 모든 프로세싱이 complete 되기 전에 그 에러 난 chunk는 다시 프로세싱을 한다고 한다. 
### 아니면 냅두고 complete 시키는 것 같다.
### <br/>

### 그리고 마지막으로 video encoding 작업을 진행한다.
#### ![image](https://github.com/Shin-jongwhan/tech_blog/assets/62974484/f740fd04-f42b-4212-9231-f8a7cda0f6ed)
### <br/>
