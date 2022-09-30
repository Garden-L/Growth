# 멀티미디어


## Analogue to Digital Conversion(ADC)
### 1. 개념
아날로그 신호를 디지털 신호로 변화.

### 2. 아날로그 신호를 디지털화하는 과정
샘플은 아날로그 신호가된다. 

#### 1. Sampling
일정 주기(T)로 아날로그 신호를 잘게 쪼갠것. S(샘플링율) = 10이라면, T=1/10sec이다.
* 주기(T) = 1/S
* Sampling Rate(S) = 1/T  


#### 2. Quantization
Sampling한 데이터를 가장 일정 수치로 해석화한 것.
![image](https://user-images.githubusercontent.com/56042451/192537529-71f088c5-b8d8-489e-961b-fd21ef9d3d9f.png)
quantization step size(△)가 작을 수록 신호의 손실을 감소 시킬 수 있다. = 비트수(bit depth)가 많으면 양자화 스텝 크기가 작아지며 신호 손실은 줄어든다. 하지만 데이터의 크기는 증가한다.

* 양자화 bit-length : bits/samples
* 양자화 : Q(s) = floor(s/△ + 0.5)
* 역양자화 : s' = △\*Q(s)

### 3. Sampling theory
샤넌이 발표한 이론으로 Sampling rate가 주파수대역폭의 2배이상이면 디지털로 변환된 아날로그 신호를 다시 아날로그신호로 복구했을 때 손실없이 복구할 수 있다는 이론이다.

#### Sampling rate(sample/sec) >= 2 * fBW(주파수 대역폭) 

인간이 들을 수 있는 주파수 대역폭은 20~20,000Hz 이므로 Sampling rate가 19980\*2, 약 40000 이상(T=1/40000)이라면, 즉 초당 40000개 이상의 샘플을 봅아내어 디지털 신호로 변환시키면 아날로그 신호로 복호 시켰을 때 손실없이 복호 시킬 수있다.

### 4. 데이터 크기
* 초당 데이터 (bytes/sec) = Sampling rate(samples/sec) * 양자화 bit-length(bytes/sample)

#### 동영상 원본 영상 크기
3 bytes/pixel, 해상도 = 7680x4320 pixels/frame, 총 영상 크기 = 3x76580x4320 bytes/frame (1프레임다 100Mbytes)
24프레임당 60분짜리 영상이라면 8.64Tera bytes/hour이기 때문에 실제로 저장하기에 많은 용량이 필요하기 때문에 *압축기술*필요
<br></br>
## 데이터 압축
