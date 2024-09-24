# Manual Chunks

블로그 프론트엔드를 드디어 배포해보려고 빌드했더니 이런 경고문이 떴다.
![image](https://github.com/user-attachments/assets/8be04868-6ae1-4a80-a7de-381b886c4263)

js 파일 사이즈가 커서 나타난 경고문으로 해결 방법도 함께 표시해주고 있다. 여담이지만 프론트엔드 최적화가 중요하다고 말만 많이 들었지 체감을 할 수 있는 경험이 없었는데 직접 배포를 시도하니 자연스럽게 알게 되고 하게 되어서 좋다.

일단 참고한 블로그 글은 [이곳](https://velog.io/@seesaw/Vite-%EC%97%90%EC%84%9C-build-chunks-%EC%82%AC%EC%9D%B4%EC%A6%88%EB%A5%BC-%EC%A4%84%EC%97%AC%EB%B3%B4%EC%9E%90)이고, lazy loading, dynamic import, manualChunks 옵션 설정의 세 가지 방법 중 manualChunks 옵션으로 처리하는 게 지금 상황에서 가장 효과적일 것으로 생각해 시도해보았다.

### 1차 시도

크기가 큰 react-icons를 별도로, 그리고 react 관련된 모듈을 묶어주었다. 다시 빌드해 보니 지정한 옵션에 따라 잘 나뉘어지는 건 확인했지만 여전히 500kb 초과한다.

```
export default defineConfig({
  ...
  build: {
    rollupOptions: {
      output: {
        manualChunks(id: string) {
          if (id.includes('react-icons')) {
            return '@icon-vender'
          }
          if (id.includes("node_modules/react/") || id.includes("node_modules/react-dom/")) {
            return "@react-vendor";
          }
        }
      }
    }
  }
});
```

![image](https://github.com/user-attachments/assets/804ea501-6106-44ea-8b6b-833f79331239)

### 2차 시도

크기를 어떻게 확인하면 좋을까 생각하다가 로컬에서 열어서 네트워크 탭을 봤다.
![image](https://github.com/user-attachments/assets/afa18a0a-9230-4409-b341-d6e2588730ce)

크기 기준으로 보면 이런데, 위지윅 에디터 라이브러리인 react-quill이랑 TOC 해시 링크에서 사용한 rehype가 꽤나 컸다. 폰트도 CDN으로 안 넣고 같이 말아서 빌드했는데 꽤 크고. 일단 react-quill은 어차피 내가 글 작성할 때 사용할 거니 굳이 처음에 서빙할 필요가 없다. 그렇게 생각하니 블로그에 접근하는 유저가 보는 페이지와 나만 접근할 페이지를 기준으로 나누면 좋겠다고 생각했다.
![image](https://github.com/user-attachments/assets/a0968383-f7f0-40d6-9f6a-86a9141ffdc6)

로딩시간 기준으로 보면 이런데, TOC에 사용하는 rehype와 github-slugger를 나누면 좋겠다고 생각했다.
![image](https://github.com/user-attachments/assets/e0f21959-8d91-4129-8963-8031fce59eb9)

```
export default defineConfig({
  ...
  build: {
    rollupOptions: {
      output: {
        manualChunks(id: string) {
          if (id.includes("axios")) {
            return "@networking-vendor";
          }
          if (
            id.includes("node_modules/react/") ||
            id.includes("node_modules/react-dom/")
          ) {
            return "@react-vendor";
          }
          if (id.includes("react-icons")) {
            return "@icon-vender";
          }
          if (id.includes("react-quill")) {
            return "@editor-vendor";
          }
          if (id.includes("rehype") || id.includes("github-slugger")) {
            return "@toc-vendor";
          }
        },
      },
    },
  },
});

```

결과는 요렇게 됐다.
![image](https://github.com/user-attachments/assets/f4824941-dd61-4014-a495-7b60f5b7a0f9)

### 3차 시도

그렇게 다 된 줄 알았는데 빌드한 걸 S3 버킷에 넣어 url로 접근해보니 에러가 떴다.
![image](https://github.com/user-attachments/assets/f38f0d0f-18f4-4b5f-a41a-1e9cd005ec8d)

manualChunks 설정 부분을 주석처리하고 한꺼번에 빌드했더니 문제 없이 잘 동작했다. chunk를 분리하다 보니 react-quill 라이브러리 의존성 문제가 생긴 모양이다.

gpt한테 물어보니 Vite 설정에서 optimizeDeps에 react-quill 라이브러리를 등록해주면 의존성 문제를 완화할 수 있다고 하여 적용해봤는데 잘 동작하지 않았다. 그래서 manualChunks 옵션 설정으로 직접 chunk 분리하지 않고 react-quill 라이브러리를 lazy loading 으로 처리해서 chunk 분리를 Vite한테 맡겼다. 아래는 그 결과물.

![image](https://github.com/user-attachments/assets/2e7e745e-4e54-4809-9a8b-f0f511443836)

그리고 잘 동작한다!

![image](https://github.com/user-attachments/assets/ce40aed1-b263-4ced-b360-9bf2686f2acc)

### 여담

테스트한다고 계속 빌드하고 S3에 기존 객체 삭제하고 다시 업로드하고를 반복하다보니 CI/CD가 왜 필요한지 느낄 수 있었다. 매우 귀찮다!! 삭제 후 업로드 aws api 쏘는 걸로 만들어서 그냥 deploy 명령어로 처리하고 싶다. 이번에 배포하면서 그냥 같이 해봐버려야겠다.
