# Mapped type

```
// Mapped type example

type Enum = 'a' | 'b' | 'c';
type MappedType = {
    [key in Enum]: {
        foo: string;
        bar: number;
    }
}
```

### Mapped type과 다른 정적으로 지정한 타입을 함께 사용하고 싶을 때

상황: theme 설정 중 variant에 따라 스타일을 다르게 지정한 Typography에서 테마 모드에 따른 기본 색상을 지정해주고 싶었다.

```
// theme.ts
const light = {
    ...
    typo:
        i18n.language === "ko"
            ? { ...typoKo, ...lightTypoColor }
            : { ...typoEn, ...lightTypoColor },
    ...
};

// emotion.d.ts
declare module "@emotion/react" {
    export interface Theme {
        ...
        typo: {
            [key in TTypoVariant]: {
                fontSize: string;
                lineHeight: string;
                letterSpacing: string;
                fontWeight: number;
            };
            defaultColor: string; // error 발생
        };
        ...
    }
}
```

`Parsing error: A mapped type may not declare properties or methods.` 에러가 발생했다.

type 키워드로 객체 형태 타입 합쳐서 지정할 때처럼 다음과 같이 해 주니 해결되었다.

```
// emotion.d.ts
declare module "@emotion/react" {
    export interface Theme {
        ...
        typo: {
            [key in TTypoVariant]: {
                fontSize: string;
                lineHeight: string;
                letterSpacing: string;
                fontWeight: number;
            };
        } & { defaultColor: string };
        ...
    }
}
```
