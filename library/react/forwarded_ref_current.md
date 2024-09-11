# forwardedRef.current 사용법

결국 필요없게 되었지만 블로그 포스트 TOC 만들면서 forwardedRef의 current값을 사용하고 싶어서 찾아본 내용.

> https://stackoverflow.com/questions/62238716/using-ref-current-in-react-forwardref

```
// useForwardRef hook

import { ForwardedRef, useEffect, useRef } from "react";

export const useForwardRef = <T>(
  ref: ForwardedRef<T>,
  initialValue: any = null
) => {
  const targetRef = useRef<T>(initialValue);

  useEffect(() => {
    if (!ref) return;

    if (typeof ref === "function") {
      ref(targetRef.current);
    } else {
      ref.current = targetRef.current;
    }
  }, [ref]);

  return targetRef;
};
```

```
// TOC

export const TOC = forwardRef<HTMLDivElement, TOCProps>({ content }, ref) => {
  const forwardedRef = useForwardRef<HTMLDivElement>(ref)

  // forwardRef로 전달받은 ref의 current값 접근 및 사용 가능
  console.log(forwardedRef.current);

  ...
}
```
