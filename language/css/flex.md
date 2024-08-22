# Flex

## flex-shrink

`flex-container`보다 `flex-item`이 더 커서 넘어갈 때 `flex-item` 크기를 줄여 `flex-container` 크기에 맞춘다.

> The flex-shrink CSS property sets the flex shrink factor of a flex item. If the size of all flex items is larger than the flex container, the flex items can shrink to fit according to their flex-shrink value. Each flex line's negative free space is distributed between the line's flex items that have a flex-shrink value greater than 0.
> [mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-shrink)

이때

```
flex-shrink: 1
```

로 주니 동작하지 않고

```
flex-shrink: 1;
overflow: 'auto';
```

로 `overflow` 처리를 해 줘야 의도대로 동작하더라.
