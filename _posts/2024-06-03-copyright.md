---
layout: post
title:  "Inserting Invisible Watermarks"
categories: [ CodeStudy ]
image: assets/img/20240603_1.png
tags: [featured]
---

# Code

## Wavelet-Based Digital Watermarking
```python
from PIL import Image
import numpy as np
import pywt


def load_image(image_path):
    return Image.open(image_path).convert("RGB")


def embed_watermark(image_path, logo_path, output_path, format, dpi=(300, 300)):
    image = load_image(image_path)
    logo = load_image(logo_path)
    image_data = np.array(image)

    watermarked_channels = []
    for channel in range(3):  # RGB 채널을 분리하여 처리
        coeffs = pywt.dwt2(image_data[:, :, channel].astype(float), 'haar')
        cA, (cH, cV, cD) = coeffs

        # 로고를 웨이블릿 변환된 cA 배열의 크기에 맞춥니다.
        resized_logo = logo.resize((cA.shape[1], cA.shape[0]))
        resized_logo_data = np.array(resized_logo)
        logo_gray = np.mean(resized_logo_data, axis=2) / 255.0  # 로고를 그레이스케일로 변환하고 정규화

        watermark_intensity = 0.05
        cA += logo_gray * watermark_intensity

        # 역 웨이블릿 변환을 채널별로 수행
        watermarked_channel = pywt.idwt2((cA, (cH, cV, cD)), 'haar')
        watermarked_channels.append(watermarked_channel)

    # 채널을 다시 결합
    watermarked_image = np.stack(watermarked_channels, axis=-1)
    watermarked_image = np.clip(watermarked_image, 0, 255).astype(np.uint8)
    result = Image.fromarray(watermarked_image, "RGB")

    # 이미지를 저장하면서 DPI 정보를 포함시킵니다.
    result.save(output_path, format=format.upper(), dpi=dpi)


# 사용 예
embed_watermark("아이패드작업/나무.jpg", "signature2.png", "after_mark/path_to_output.jpg", "JPEG")
embed_watermark("아이패드작업/나무.png", "signature2.png", "after_mark/path_to_output.png", "PNG")
embed_watermark("아이패드작업/나무.tiff", "signature2.png", "after_mark/path_to_output.tiff", "TIFF")
```  

이 코드는 디지털 이미지에 웨이블릿 변환을 사용하여 비가시적 워터마크를 삽입하는 프로세스를 구현합니다. 자세히 살펴보겠습니다.

### 1. 함수 및 라이브러리 설명

- **PIL (Python Imaging Library)**: 이 라이브러리는 이미지 파일을 열고 조작하는 기능을 제공합니다.
- **numpy**: 고성능 수치 계산을 위해 배열 및 행렬 연산을 지원하는 라이브러리입니다.
- **pywt (PyWavelets)**: 웨이블릿 변환을 수행할 수 있게 해주는 라이브러리입니다.

### 2. `load_image` 함수

이 함수는 이미지 파일 경로를 받아서 해당 이미지를 열고, RGB 형식으로 변환하여 반환합니다. RGB 형식으로 변환하는 이유는 이미지의 색상 데이터를 일관되게 처리하기 위함입니다.

```python
def load_image(image_path):
    return Image.open(image_path).convert("RGB")
```

### 3. `embed_watermark` 함수

이 함수는 워터마크를 삽입할 메인 이미지 경로, 로고 이미지 경로, 출력 파일 경로, 출력 형식, DPI 설정을 인자로 받습니다.

#### 이미지와 로고 불러오기

- `image`: 원본 이미지 데이터
- `logo`: 워터마크로 사용할 로고 이미지 데이터

```python
image = load_image(image_path)
logo = load_image(logo_path)
```

#### 워터마킹 처리

- 원본 이미지는 RGB 채널로 분리되어 각 채널에 대해 독립적으로 웨이블릿 변환을 적용합니다.
- `pywt.dwt2` 함수는 2차원 웨이블릿 변환을 수행하고, 결과는 저주파수 성분(`cA`)과 세 가지 고주파수 성분(`cH, cV, cD`)으로 구성됩니다.

```python
coeffs = pywt.dwt2(image_data[:, :, channel].astype(float), 'haar')
cA, (cH, cV, cD) = coeffs
```

- 로고 이미지는 원본 이미지의 저주파수 성분 크기에 맞게 크기를 조정하고, 그레이스케일로 변환 후 정규화합니다.

```python
resized_logo = logo.resize((cA.shape[1], cA.shape[0]))
resized_logo_data = np.array(resized_logo)
logo_gray = np.mean(resized_logo_data, axis=2) / 255.0
```

- 정규화된 로고 데이터를 원본 이미지의 저주파수 성분에 추가하여 워터마크를 삽입합니다. `watermark_intensity`는 워터마크의 강도를 조절합니다.

```python
cA += logo_gray * watermark_intensity
```

- 웨이블릿 변환의 역과정을 수행하여 워터마크가 삽입된 새로운 이미지 채널 데이터를 생성합니다.

```python
watermarked_channel = pywt.idwt2((cA, (cH, cV, cD)), 'haar')
```

- 모든 채널을 다시 결합하고, 색상 값이 0에서 255 사이가 되도록 조정합니다.

```python
watermarked_image = np.stack(watermarked_channels, axis=-1)
watermarked_image = np.clip(watermarked_image, 0, 255).astype(np.uint8)
result = Image.fromarray(watermarked_image, "RGB")
```

#### 결과 저장

- 최종 이미지는 주어진 형식과 DPI로 저장됩니다.

```python
result.save(output_path, format=format.upper(), dpi=dpi)
```

### 사용 예

- `embed_watermark` 함수를 호출하여 다양한 이미지 형식(JPEG, PNG,

 TIFF)에 대해 워터마크를 삽입하는 예시입니다.

이 코드는 각 단계에서 이미지의 RGB 채널을 독립적으로 처리하고, 웨이블릿 변환을 이용하여 워터마크를 비가시적으로 삽입하는 고급 기술을 사용합니다. 이러한 워터마크는 일반적으로 시각적으로 확인이 어렵지만, 적절한 도구를 사용하여 추출하거나 검증할 수 있습니다.
  
  
<br>

## Watermark Detection Code
```python
import numpy as np
import pywt
from PIL import Image
import matplotlib.pyplot as plt

def load_image(image_path):
    return Image.open(image_path).convert("RGB")

def detect_watermark(watermarked_image_path, original_logo_path):
    # 워터마크가 삽입된 이미지 로드
    watermarked_image = load_image(watermarked_image_path)
    watermarked_data = np.array(watermarked_image)

    # 로고 이미지 로드 및 준비
    original_logo = load_image(original_logo_path)
    original_logo = original_logo.convert("L")  # 그레이스케일로 변환

    watermarked_channels = []
    logo_channels = []
    for channel in range(3):  # RGB 채널을 분리하여 처리
        coeffs = pywt.dwt2(watermarked_data[:, :, channel].astype(float), 'haar')
        cA, (cH, cV, cD) = coeffs

        # 로고 이미지를 웨이블릿 변환된 cA 배열의 크기에 맞춥니다.
        resized_logo = original_logo.resize((cA.shape[1], cA.shape[0]))
        resized_logo_data = np.array(resized_logo) / 255.0

        watermarked_channels.append(cA)
        logo_channels.append(resized_logo_data)

    # 워터마크 강도
    watermark_intensity = 0.05

    # 워터마크 패턴 추출
    watermark_pattern = np.mean(watermarked_channels, axis=0) - np.mean(logo_channels, axis=0) * watermark_intensity

    plt.figure(figsize=(10, 5))
    plt.subplot(1, 2, 1)
    plt.title("Detected Watermark Pattern")
    plt.imshow(watermark_pattern, cmap='gray')
    plt.colorbar()
    plt.subplot(1, 2, 2)
    plt.title("Original Logo")
    plt.imshow(np.mean(logo_channels, axis=0), cmap='gray')
    plt.colorbar()
    plt.show()

# 사용 예
detect_watermark("after_mark/path_to_output.jpg", "signature2.png")
```


<br>
 
![1]({{ site.baseurl }}/assets/img/20240603_1.png)    