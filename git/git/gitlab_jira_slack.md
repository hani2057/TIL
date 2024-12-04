# Git hooks - Gitlab, Jira, Slack 연동

## 상황

Gitlab merge request 생성될 때마다 Slack에 코드 리뷰 요청 메시지를 송부하고 있는데, 자동화해 보고 싶다.

## 조건

- Gitlab 사용중
- 브랜치명은 Jira 티켓 번호
- Jira 티켓 타이틀 작성 필요
- 팀원 태그 필요

## 생각

1. Gitlab webhook을 이용해서 mr 생성을 감지할 수 있다. [Gitlab Webhooks](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html)
2. Slack webhook을 이용해서 메시지를 송부할 수 있다. [Slack webhooks](https://api.slack.com/messaging/webhooks)
3. 브랜치명이 이슈 티켓 번호니까 추출해서 Jira에서 api로 검색하면 티켓 제목을 가져올 수 있지 않을까?

## 테스트

### 1. Slack에 메시지 보내기

일단 테스트용 앱을 하나 만들고, 테스트용 채널을 만들어서 Incoming webhook을 연결했다.

(여기는 캡쳐와 설명이 없음... 귀찮으니 생략해두고 나중에 추가하든지 해야지. [요기](https://velog.io/@king/slack-incoming-webhook)와 [요기](https://thisisjoos.tistory.com/752) 정도 참고하면 정보는 충분하다.)

api 호출!

```
curl -X POST --data-urlencode "payload={\"channel\": \"test\", \"username\": \"test-user\", \"text\": \"테스트\"}" https://hooks.slack.com/services/(내 테스트용 url 주소)
```

잘 된다!
![Screenshot 2024-11-27 at 9 32 08 PM](https://github.com/user-attachments/assets/a60f94af-b766-4fef-a252-c7c2b36382f9)

그래서 mr-review-request-bot 이라는 이름으로 양식을 써서 보내 봤는데 이것도 잘 갔다!

### 2. 웹훅에 사용자 태그 추가

[스택오버플로우](https://stackoverflow.com/questions/47491331/mentioning-users-via-slack-in-webhooks)에서 `<@memberID>`를 웹훅에 실어 보내면 멘션이 된다고 해서 내 아이디를 넣어봤는데 멘션도 잘 된다!

유저 아이디는 이렇게 확인하면 된다.
![Screenshot 2024-11-27 at 9 35 24 PM](https://github.com/user-attachments/assets/230eeb46-655e-4488-950e-36ab0becb4ee)

### 3. 깃랩에서 mr 생성 이벤트 감지해서 git hook 생성

여기 굉장히 애를 먹었다ㅠㅠ

일단 정리하자면,

1. 기본적으로 깃에서 훅을 제공한다. vscode에서 `.git` 폴더 보이게 한 다음 눌러 보면 깃이 기본적으로 제공하는 훅들이 예시와 함께 잘 정리되어 있다. 자세한 건 [깃 공식문서](https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-Hooks)를 참고. 깃 훅은 클라이언트 훅(pre-event-hook)이 있고 서버 훅(post-event-hook)이 있는데, pre-commit 등 보통 husky로 컨트롤해서 사용하는 것들이 클라이언트 훅이고 push 전후로 실행되는 게 서버 훅이다.
2. 깃랩에서 슬랙으로 이벤트를 감지하여 메시지를 보내거나, 슬랙에서 깃랩으로 명령어를 이용해 이벤트를 트리거하는 건 깃랩과 슬랙이 이미 잘 해뒀다. 나처럼 커스텀을 하고 싶은 게 아니라 잘 만들어둔 거 잘 쓰고 싶다고 하면 가이드문서도 잘 만들어져있으니 [여기](https://docs.gitlab.com/ee/user/project/integrations/gitlab_slack_application.html)를 참고하면 된다.
3. 깃랩에서 웹훅을 사용하려면 프로젝트 세팅에서 웹훅을 추가하면 된다. 웹훅 이벤트 감지했을 때 호출할 url을 넣는 아주 간단한 방식인데, 나는 그 url 제공에서 애를 먹었다.

일단 깃랩 웹훅 사용 방법은 [깃랩 공식문서 여기](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html)를 보면 된다.

그리고 웹훅이 post 요청이라서 서버가 필요하다. 나는 서버 배포를 해 본 적이 없어서 우여곡절 끝에 vercel에서 express 사용한 배포 설명해둔 문서랑 샘플로 제공한 깃헙 레포를 찾아서 겨우 할 수 있었다. 설명해둔 공식 문서는 [여기](https://vercel.com/guides/using-express-with-vercel), 샘플 깃헙 레포 주소는 [여기](https://github.com/vercel/examples/blob/main/solutions/express/api/index.ts)이다.

코드는 이렇게 된다.

```
// app.js

const axios = require("axios");
const express = require("express");
const app = express();

app.use(express.json());

app.post("/api", async (req, res) => {
  if (!req.body) {
    return res.status(400).send("request information is missing");
  }

  const payload = {
    channel: "#test",
    username: "mr-review-request-bot",
    text: "test",
  };

  try {
    const slackRes = await axios.post(
      slackHookUrl,
      JSON.stringify(payload)
    );
    if (slackRes.status === 200) {
      res.status(200).send("Message sent to slack");
    }
  } catch (err) {
    console.error(err);
    res.status(slackRes.status).send("Failed to send message to slack");
  }
});

app.listen(3000, () => console.log("Server ready on port 3000."));

module.exports = app;
```

```
// vercel.json
{
  "version": 2,
  "rewrites": [{ "source": "/(.*)", "destination": "/api" }]
}
```

test 채널에 영롱하게 메시지가 잘 보내졌다!
~~그리고 슬랙에 메시지 보내는 url을 잘못 복사해서 한참동안 삽질한 건 그냥 잊자ㅠㅠㅠ~~

### 4. 깃랩 웹훅에서 가져온 이슈티켓 정보로 Atlassian 검색 api 호출

깃랩 웹훅에서 기력을 심히 소모하여 공식문서(영어)가 잘 안 읽히던 중 [아주아주 친절하게 정리해두신 블로그 글](https://myung-ho.tistory.com/81)을 발견했다.

회사 계정으로 바로 토큰 발급받아서 api 문서 중에 issue search를 찾아 이거구나! 하고 curl로 바로 쏴봤는데, 요청은 잘 가고 응답도 잘 오는데 검색 결과가 비어 있었다.

왜인지 찾다가, 아무리 봐도 v2의 [이 api](https://developer.atlassian.com/server/jira/platform/rest/v10002/api-group-search/#api-api-2-search-get)가 맞는 것 같아서 jql을 써 보기로 했다. 그런데 아틀라시안도 공식문서를 참 못 쓴다. 오히려 api는 분류를 잘 해 둬서 편했는데 가이드문서에 죄다 예시만 적어 둬서 파라미터 정보 모아둔 페이지를 찾아 찾아 타고 들어갔다. [여기](https://support.atlassian.com/jira-service-management-cloud/docs/jql-fields/)를 보면 된다.

그래서 결론적으로 아틀라시안 api로 지라 이슈 티켓을 검색하는 함수는 이렇게 되었다.

```
...
const res = await axios({
  method: "get",
  url: `${ATLASSIAN_URL}/rest/api/2/search`,
  params: {
    jql: `issueKey = ${issue}`,
    fields: "summary",
  },
  headers: {
    Authorization: `Basic ${Buffer.from(
      `${ATLASSIAN_USERNAME}:${TOKEN}`
    ).toString("base64")}`,
    Accept: "application/json",
  },
})
...
```

우리는 티켓 번호로 모두 관리하고 있어서 issueKey로 검색했다. url 자리에는 본인 아틀라시안 도메인을, username 자리에는 이메일 주소를, token 자리에는 아까 발급받은 토큰을 넣으면 된다. api 문서에 잘 나와 있다.

그리고 검색 잘 된다! 나는 이슈 제목만 필요해서 파라미터에 fields 값 넣어서 가져왔다.

캡쳐를 넣고 싶은데 회사 정보가 들어가 있어서 모자이크 처리하기 귀찮아서 그냥 캡쳐는 생략... 사실 깃랩 웹훅부터 그랬다.

아무튼 한 mr에 이슈가 여러 개 있을 수 있으니 Promise.allSetteld로 이슈를 전부 검색해서 추가했다.

대충 이런 식이다.

```
...
  // MR title, source branch, description에서 지라 티켓 이슈 넘버를 조회
  const str =
    object_attributes.title +
    object_attributes.source_branch +
    object_attributes.description;
  const matches = str.match(/KEY-\d{4}/g);
  const issues = [...new Set(matches)];

  if (issues.length === 0) {
    return res.status(400).send("No Jira issue to search");
  }

  try {
    // Atlassian Jira 이슈 검색 API 호출 함수
    const jiraIssueSearch = issues.map((issue) =>
      axios({
        method: "get",
        url: `${ATLASSIAN_URL}/rest/api/2/search`,
        params: {
          jql: `issueKey = ${issue}`,
          fields: "summary",
        },
        headers: {
          Authorization: `Basic ${Buffer.from(
            `${ATLASSIAN_USERNAME}:${TOKEN}`
          ).toString("base64")}`,
          Accept: "application/json",
        },
      })
    );
    const jiraSearchRes = await Promise.allSettled(jiraIssueSearch);
    const searchedIssues = jiraSearchRes.reduce((acc, cur) => {
      if (cur.status === "fulfilled") {
        return [...acc, ...cur.value.data.issues];
      }
      return acc;
    }, []);
    if (searchedIssues.length === 0) {
      return res.status(404).send("No Jira issue found");
    }

    // 검색된 이슈들을 슬랙 메시지에 추가
    searchedIssues.forEach((issue) =>
      slackMessage.issues.push(
        `${addUrlToIssue(issue.key)} ${issue.fields.summary}`
      )
    );
  } catch (err) {
    console.error("Error searching Jira:", err);
    return res.status(jiraSearchRes.status).send("Error searching Jira");
  }
```

그럼 이제 끝났다!

1. 깃랩 mr이 생성되면
2. 웹훅에서 내가 등록한 url로 콜백을 호출할 거고,
3. 그럼 내가 올려둔 서버에서 해당 POST 요청의 body에서 필요한 정보를 꺼내서
4. 아틀라시안 api로 이슈 검색한 다음
5. 슬랙에 보낼 메시지를 작성해서
6. 슬랙 웹훅으로 쏘면 된다!

물론 그 사이에 mr이 생성될 때, 타겟 브랜치가 develop일 때 등등 제한조건을 걸거나 슬랙 문법으로 링크를 걸거나 태그할 사람을 지정하거나 등등의 작업이 있는데 이건 필요에 따라 맞춰서 진행하면 된다.

아 그리고 깃랩 웹훅 커스텀이 가능해서, 필요한 정보만 받을 수도 있으니 참고. 웹훅 설정의 가장 아래쪽에 있고 설명되어 있는대로 작성하면 된다. 커스텀 영역에 보낼 파라미터를 작성하면 req에 해당 파라미터 정보들만 간다. (추가로 가는 게 아니다!) 사용하는 정보가 몇 개 없고 커스텀한 값을 보내고 싶을 때 유용하고 깔끔하게 코드를 쓸 수 있다는 장점이 있다. 다만 뭔가 더 필요해지면 그때마다 웹훅에 추가해줘야 하는 게 좀 불편하긴 할 거 같다.

## 결과

이 정도까지 된 걸 테스트한 결과랑 같이 본부장님께 보여드렸고, 적용해보자고 하셔서 적용 예정이다! 다만 프로젝트가 많아서 웹훅을 일일이 설정하면 너무 귀찮으니 깃랩 전체에 적용되는 시스템 훅으로 설정해보면 어떻겠냐고 하셔서 조금 수정해서 진행했다. (스크립트 돌리는 서버도 회사 서버로 옮겼다)

시스템 훅은 무려 req DTO를 적어뒀다! ~~일반 웹훅은 그런 거 없어서 실제로 보내 보고 꺼내서 썼는데ㅠ~~ [여기](https://docs.gitlab.com/ee/administration/system_hooks.html#merge-request-events)를 보면 된다.

주는 정보가 좀 다르다. 예를 들면 프로젝트에 웹훅 설정하면 `object_attributes.action` 값으로 생성인지 수정인지 머지인지 닫은건지 등등을 알 수 있는데, 시스템 훅에서는 action 필드를 제공하지 않는다. 나는 mr 생성때만 스크립트를 돌리고 싶어서 `object_attributes.created_at` 이랑 `object_attributes.updated_at` 을 비교해서 처리했다.

## 소감

일단 자동화되니 굉장히 편하다! 그리고 이번이 내 거의 첫 DX 개선 경험이다. 물론 터미널 alias 설정해서 쓰는 등 로컬에서는 몇 번 해 봤어도 팀에 유의미하게 긍정적인 개발자 경험을 제공하는 점이 좋았다. 그리고 "아 귀찮아"를 처리하기 위해 자동화하려고 끙끙대며 고민하는 '진짜 개발자 밈'의 정석같은 행동을, 마음에서 우러난 귀찮음과 "아오 될 거 같은데"로 해냈다는 점에서 개발자 경험치가 쌓인 듯해 뿌듯하다ㅎㅎㅎ
