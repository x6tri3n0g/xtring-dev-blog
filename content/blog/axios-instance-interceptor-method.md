+++ 
title= "Axios instance interceptor 메서드 알아보기" 
date= 2025-04-14
tags= ["post"] 
draft=false 
+++

# Axios instance interceptor 메서드 알아보기

> Create on: 2025.04.14

## 배경

[지수님의 La-CMS 운영 이슈 해결사항](https://github.com/thefarmersfront/la-cms-front-react/pull/5530/files)을 확인 중 [axios](https://axios-http.com/kr/docs/intro)의 instance 메서드를 확인해보게 되었다.

## axios.interceptor

`axios` 에서 인스턴스를 사용하면, 요청(req) 혹은 응답(res)이 실제 네트워크 통신을 거치기 전후에 원하는 ㅇ로직을 삽입하여 처리할 수 있다. 일반적으로 아래의 상황에서 사용된다.

- 공통적인 헤더, 토큰, 인증 정보를 요청에 추가할 때
- API 응답 형식을 통일하여 에러 처리를 자동화할 때
- 응답을 가공하여 애플리케이션 상태에 반영할 때

### 예시

```js
import axios from "axios";

// 1. axios 인스턴스 생성
const apiClient = axios.create({
  baseURL: "https://example.com/api", // 다른 설정들...
});

// 2. 요청(Request) 인터셉터 설정
apiClient.interceptors.request.use(
  function (config) {
    // 요청 직전 공통적으로 처리할 로직
    // 예: 요청 헤더에 토큰 추가
    const token = localStorage.getItem("accessToken");
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  function (error) {
    // 요청 에러가 발생했을 때 처리할 로직
    return Promise.reject(error);
  }
);

// 3. 응답(Response) 인터셉터 설정
apiClient.interceptors.response.use(
  function (response) {
    // 응답 데이터를 가공하거나
    // 상태코드에 따라 처리할 로직
    return response;
  },
  function (error) {
    // 응답 에러가 발생했을 때 처리할 로직
    if (error.response && error.response.status === 401) {
      // 예: 인증 에러 처리
      // 로그인 페이지로 리다이렉트 또는 토큰 갱신 요청
    }
    return Promise.reject(error);
  }
);

// 이제 apiClient를 사용해서 요청을 보냄
apiClient
  .get("/users")
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error(error);
  });
```

### interceptors.request.use(onFulfilled, onRejected)

- **onFulfilled**
  - 요청이 전송되기 _직전_ 에 실행됩니다.
  - config 객체를 반환하면, 이 config 정보가 실제 서버로 전송됩니다.
  - 예: 토큰 추가, 공통 헤더 설정, 요청 파라미터 가공 등.
- **onRejected**
  - 요청 과정에서 에러가 발생했을 때 실행됩니다.
  - 예: 요청을 보내기 전에 필수 토큰이 없을 경우 에러 처리.

### interceptors.response.use(onFulfilled, onRejected)

- **onFulfilled**
  - 서버로부터 정상적으로 응답이 온 후(HTTP 상태 코드 2xx) 실행됩니다.
  - 응답 데이터를 가공하거나, 로그용으로 추가 정보를 처리할 수 있습니다.
- **onRejected**
  - 응답 과정에서 에러가 발생(HTTP 상태 코드 4xx, 5xx 등)했을 때 실행됩니다.
  - 에러 메시지 안내, 재시도 로직, 토큰 갱신 등을 처리할 수 있습니다.

### 인터셉터의 체인 구조

- Axios는 요청 → 응답 과정에서 onFulfilled/onRejected 함수를 체인 형태로 순차 실행합니다.
- 이 말은 여러 개의 인터셉터를 등록할 수 있으며, 순서대로 실행된다는 뜻입니다.
- 요청 시에는 **등록 순서대로** 실행되고, 응답 시에는 **역순**으로 실행됩니다.
