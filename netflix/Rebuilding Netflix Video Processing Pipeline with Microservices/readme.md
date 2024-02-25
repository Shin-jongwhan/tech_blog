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
