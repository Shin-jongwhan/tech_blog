### 240225
## [Rebuilding Netflix Video Processing Pipeline with Microservices](https://netflixtechblog.com/rebuilding-netflix-video-processing-pipeline-with-microservices-4e5e6310e359)
### [High Quality Video Encoding at Scale](https://netflixtechblog.com/high-quality-video-encoding-at-scale-d159db052746) 내용을 토대로 그동안 발전된 video processing 파이프라인, microservice orchastration 내용을 담고 있다.
### <br/><br/><br/>

## video processing boundaries
### netflix는 다음과 같이 영역을 나누었고 다음의 순서로 video processing을 이전보다 효과적으로 진행할 수 있게 되었다.
1. divide the input video into small chunks
2. encode each chunk independently
3. calculate the quality score (VMAF) of each chunk
4. assemble all the encoded chunks into a single encoded video
5. aggregate quality scores from all chunks
### <br/><br/><br/>

## video service(processing)
### 이전 블로그인 [High Quality Video Encoding at Scale](https://netflixtechblog.com/high-quality-video-encoding-at-scale-d159db052746) 내용을 정리하고 추가하여 각각의 프로세싱 명칭을 정하였다.
1. Video Inspection Service (VIS)<br/>
    - downstream services를 위해 input, metadata를 검사한다.
<br/>

2. Complexity Analysis Service (CAS)
    - mezzanine(이중층)이라고 표현하는데, 전체를 프로세싱 하기 전에 미리 검사하는 것이다. to understand the content complexity이라고 설명하였고
    - pre-encoding이라고 한다(It calls Video Encoding Service for pre-encoding and Video Quality Service for quality evaluation).
    - 이렇게 processing된 data는 database에 저장되고 re-used할 수 있다고 한다.
<br/>

3. Ladder Generation Service (LGS)
    -  다양한 encoding family를 지원하기 위해 entire bitrate ladder라는 걸 만든다.
    -  optimization algorithm to create encoding recipes
    -  종합하자면, 다양한 encoding 방식을 지원하고 최적화하는 작업을 해준다.
<br/>

4. Video Encoding Service (VES)
    - LGS에서 넘어온 것을 encoding해주는 작업. 여기서는 위에서 소개한 이전 블로그를 참고하면 좋다.
    - 병렬 처리가 이루어진다.
<br/>

5. Video Validation Service (VVS)
    - encoding된 파일이 우리가 기대한 encoding 방식이 맞는지 확인한다.
    - 아니면 alert 해준다.
<br/>

6. Video Quality Service (VQS)
    - video encoding이 끝나면 quality score(VMAF)를 매기고 평가한다.
<br/>
### <br/><br/><br/>

## Service Orchestration
### 이 주제의 첫 문장이 이것이다.
#### Each video service provides a dedicated functionality and they work together to generate the needed video assets.
### 이게 가장 의미 있는 문장이 아닐까 한다. 왜냐면 각각의 서비스들이 제 역할에 맞춰 운영이 되고 있다고 하고, 각 서비스들은 또한 잘 연결이 되어 있어 전체적으로 원활한 파이프라인 구성을 하고 있다는 의미가 되기 때문이다.
### 이렇게 분업화되고 협업이 잘 이루어져야 모든 프로세싱이 잘 돌아간다.
### <br/>

### video는 streaming을 위해 content delivery network (CDN)로 deploy된다.
#### These videos can easily be watched millions of times. 
### orchastrator는 다양한 작업을 수행한다.
### orchastrator는 streaming을 위해 audio, text 등을 packaging 해서 containerize 하여 사용한다.
#### It leverages VIS to detect and reject non-conformant or low-quality mezzanines, invokes LGS for encoding recipe optimization, encodes video using VES, and calls VQS for quality measurement where the quality data is further fed to Netflix’s data pipeline for analytics and monitoring purposes. In addition to video services, the Streaming Workflow Orchestrator uses audio and timed text services to generate audio and text assets, and packaging services to “containerize” assets for streaming.
### <br/>

### video pipeline은 크게 두 가지가 있다. 그리고 전체적인 workflow는 아래 모식도를 참고한다.
### orchastrator가 전체적인 조율을 담당하고 있다.
#### the two main use cases of the Netflix video pipeline are producing assets for member streaming and for studio operations.
#### ![image](https://github.com/Shin-jongwhan/tech_blog/assets/62974484/8dcf6661-47b0-46d3-97ec-4d8fd0418c43)
### <br/>
