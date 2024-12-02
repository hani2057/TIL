# 서버 없이 브라우저에서 외부 api 호출하기 (Vite proxy 설정 + vercel.json 설정)

### 상황

1. oauth 인증해서 외부 서비스 이용하는 과정 테스트 중이다.

2. POC니까 간단하게 하려고 DB는 로컬스토리지로 대체하고 클라이언트에서만 http 요청으로 처리하려고 했다.

3. 유튜브는 별 문제 없이 되었는데, 인스타그램은 리다이렉트 url 설정에 로컬호스트를 지원하지 않아서 배포 환경에서 테스트해야 했다.

### CORS 에러 임시 해결을 위한 proxy 설정

원칙적으로는 CORS 에러는 보안을 위한 브라우저 기본 정책이므로 서버에서 allow origin으로 설정해서 통신해야 한다. 하지만 지금은 POC를 얼른 만들고 싶어서 임시로 클라이언트에 프록시 서버를 설정해 줬다. (oauth 처음 해 볼 때 삽질하면서 express로 서버도 만들었었는데 그냥 클라이언트에서 하는 게 편하긴 하다 나는 프론트니까^^ 사실 백엔드 아직 잘 몰라서 그나마 익숙한 곳에서 해결하는 게 편하더라)

vite는 프록시 설정 기능을 지원해줘서 vite.config.json에 설정 추가하면 된다. [vite 공식문서](https://ko.vite.dev/config/server-options#server-proxy)

```
// vite.config.json

import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      "/instagram-short": {
        target: "https://api.instagram.com/oauth/access_token",
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/instagram-short/, ""),
      }
    },
  },
});

```

`"/instagram-short"` 로 보내는 api를 `"https://api.instagram.com/oauth/access_token"`로 바꿔서 보내겠다는 뜻이다.

설정을 해 줬으니 기존에 `"https://api.instagram.com/oauth/access_token"` 로 보내고 있던 코드를 `'/instagram-short'` 로 보내도록 수정한다.

```
...
const shortAccessTokenRes = await axios({
        method: "POST",
        url: "/instagram-short", // 여기가 원래 "https://api.instagram.com/oauth/access_token" 였다.
        data: {
          client_id: CLIENT_ID,
          client_secret: CLIENT_SECRET,
          grant_type: "authorization_code",
          redirect_uri: `${REDIRECT_BASE_URL}/instagram/redirect`,
          code: code?.split("#")[0],
        },
      });
...
```

그러면 기존에는 내 배포 환경 origin에서 인스타그램 인증 서버로 요청을 보내고 있던 api 호출이, 프록시 서버를 거쳐 내 배포 환경 origin을 인스타그램 인증 서버와 동일한 origin으로 바꿔서 호출해주기 때문에 cors 에러를 해결할 수 있다.

로컬에서 테스트해 보니 잘 된다.
![스크린샷 2024-12-02 오후 12 15 08](https://github.com/user-attachments/assets/4faa895a-9b98-40eb-ab5e-f34e099d5986)

인스타그램은 redirect url에 로컬호스트를 지원하지 않아서 배포환경에서만 테스트 가능하다. (매우 귀찮다... 사실 귀찮아서 코드 다 짜고 배포하면서 상상코딩 실력이 늘었다.)

잘 되는 걸 봤으니 main에 푸시해서 배포하자! vercel 최고!

그런데 처음 보는 에러가 도착했다.
![스크린샷 2024-12-02 오전 11 55 25](https://github.com/user-attachments/assets/b54c395a-0615-4765-8305-a6ce51109f10)

405는 진짜 처음 봤다. 해당 주소에 대해 내가 보낸 POST 요청이 허락되지 않았다 즉 서버에서 지원하지 않는다는 내용 같아서 mdn 보니 맞더라. [mdn 405](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/405)

근데 request url을 보면 프록시가 적용이 안 되어 있다. 왜인지 찾아보다가 [스택오버플로우](https://stackoverflow.com/questions/76921210/proxy-is-not-working-in-vite-js-project-and-request-is-not-getting-redirected-to)에서 원인을 찾았다. 스택오버플로우 최고!
![스크린샷 2024-12-02 오후 12 25 00](https://github.com/user-attachments/assets/b918cd19-a923-4dd2-ae26-f10c2f087626)

사실 공식문서를 잘 읽는 게 좋은데 틈틈이 읽으려고 해도 읽을 게 너무 많아서 힘들다... react랑 typescript도 다 못 읽었는데 vercel을 언제 읽고 앉아있냐고...

### vercel.json 에 rewrites 설정

아무튼, 저기 적혀 있는 대로 vite proxy 설정은 데브 서버에만 적용되고 배포 환경에서는 동작하지 않는다. 그래서 vercel.json에 아래와 같이 `"rewrites"`를 추가해주었다.

```
{
  "rewrites": [
    {
      "source": "/instagram-short",
      "destination": "https://api.instagram.com/oauth/access_token"
    },
    {
      "source": "/instagram-long",
      "destination": "https://graph.instagram.com/access_token"
    },
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

참고로 vercel 배포했을 때 루트 경로를 못 찾길래 vercel.json에서 `routes` 설정을 해 뒀었는데, [이런 이유](https://vercel.com/docs/errors/error-list#mixed-routing-properties) 때문에 기존에 라우팅 설정 해 줬던 `routes` 옵션은 `rewrites` 로 포함시켜주어야 한다.

배포해보니 잘 된다!

~~다른 에러가 떴을 뿐^^~~

다만 request url은 프록시 설정하거나 rewrite해도 처음 보내려고 작성한 주소로 표시되는 모양이다. 몰랐네...
