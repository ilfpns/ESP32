> ReadMark에서 사용된 Cmake를  가져왔다
> 
> 
> cMake는빌드 구성을 생성한다
> 

```
idf_component_register(
    SRCS "main.c" "wifi.c" "camera.c" "http.c"
    INCLUDE_DIRS "."
    REQUIRES esp_wifi wpa_supplicant nvs_flash esp_event esp_netif esp_http_client json
)
```

## SRCS

⇒ 해당 컴포넌트를 빌드할 때 어떤 파일을 포함시킬지 정함

동작 과정은 다음과 같다

1. CMake가 `SRCS` 안의 파일들을 전부 컴파일 대상으로 등록
2. 각 소스는 `.o` (object file)로 변환됨
3. 이 컴포넌트 단위의 라이브러리(`lib<컴포넌트명>.a`)로 묶임
4. 나중에 링크 단계에서 메인 실행파일(`.elf`)에 포함

## INCLUDE_DIRS

⇒ 컴파일 할 헤더 파일을 찾아 디렉토리 경로를 지정

- 만약 사용자가 `include <stdio.h>` 를 썼다면
- 컴파일러가 저 헤더를 어디서 찾을지 알려주는 검색 경로이다

---

`INCLUDE_DIRS "." "include”` 

다음과 같은 경로라면

1. main.c가 있는 파일을 포함하고 (.) 
2. include폴더도 헤더 검색 경로에 포함한다

다른 컴포넌트와 연계

- INCLUDE_DIRS
    - 이 컴포넌트를 사용하는 다른 컴포넌트에서도 해당 경로를 헤더 검색 경로에 추가해줌.
    - 즉, 공개 헤더(public header) 지정.
- PRIV_INCLUDE_DIRS
    - 이 컴포넌트 내부에서만 쓰이는 헤더 경로.
    - 외부에서 접근 불필요할 때 사용.

## REQUIRES

⇒ 컴포넌트 의존성을 나타내는 키워드

- ESP-IDF는 각 기능(wifi, camera ..)을 컴포넌트로 관리한다
- 내가 만든 컴포넌트가 다른 컴포넌트를 쓰려면 Cmake에서 링크하면 된다

---

cMake안에서 헤더들을 `include` 하고 API를 작성하면 

→ cMake가 해당 컴포넌트를 링크하고 빌드한다

- REQUIRES
    - 공용 의존성이라 불림
    - 다른 컴포넌트를 사용할 때도 의존성이 따라감
    - 헤더 include path도 외부에 공개
    
- PRIV_REQUIRES
    - 개인 의존성
    - 현재 컴포넌트 내부에서만 필요한 경우
    - 외부에서 이 컴포넌트를 쓸 때 노출되지 않음

다음과 같이 쉽게 비유할 수 있다

- **REQUIRES** → “나랑 같이 쓰려면 얘네도 꼭 필요해. 같이 가져가.”
- **PRIV_REQUIRES** → “내부적으로만 얘네가 필요해. 너한텐 안 보여줘도 돼.”
