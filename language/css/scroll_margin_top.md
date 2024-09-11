# scroll-marin-top CSS 속성

개발블로그 만들면서 해시 링크로 TOC 만들고 있었는데, href로 id값 찾아 이동할 때 브라우저 최상단으로 해당 타이틀이 위치하도록 이동해버려서 GNB에 가려지는 상황이 발생했다. 방법을 찾아보다가 이미 잘 되어 있는 사이트 한 번 뜯어보라고 팀원이 조언해주셔서 mdn 문서에서 확인해보니 scroll-margin-top이라는 딱 봐도 내가 찾는 것 같은 속성명이 보였다.

![image](https://github.com/user-attachments/assets/595f9ffc-9d9a-42ad-919d-cb46221a87bb)

<br />

> The scroll-margin-top property defines the top margin of the scroll snap area that is used for snapping this box to the snapport. The scroll snap area is determined by taking the transformed border box, finding its rectangular bounding box (axis-aligned in the scroll container's coordinate space), then adding the specified outsets.
> [mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-margin-top)

유레카!
