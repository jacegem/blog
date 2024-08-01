---
title: '[Next.js] tailwind css 테마 설정'
date: 2024-08-01
tags: [next.js, tailwin, css]
categories:
  - Framework
  - Next.js
---

![](https://i.imgur.com/KlpAFq1.png)

다크 테마로 만들어본다.

### `lib/style/dark-green.css` 파일을 생성

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
	.dark {
		--background: #071623;
		--background-on: #ffffff;
		--background-sub: #b3b8ad;
		--primary: #bdf75c;
		--primary-on: #090909;
		--secondary: #f08f48;
		--secondary-on: #fff;
		--card: #1f2f39;
		--card-on: #ffffff;
		--card-sub: #a4a9a1;
		--ring: #bdf75c;
		--border: #b3b8ad;
		--input: #b3b8ad;
		...;
	}
}
```

### `app/providers.tsx` 파일에 css import 를 추가

```typescript
'use client'

import '@/lib/style/dark-green.css'
import { ThemeProvider } from 'next-themes'

export default function Providers({ children }: { children: React.ReactNode }) {
	return (
		<ThemeProvider attribute="class" defaultTheme="dark">
			{children}
		</ThemeProvider>
	)
}
```

ThemeProvider는 `app/layout.tsx` 파일에서 import 합니다.

### tailwind.config.ts 파일 설정

```typescript
const config = {
  darkMode: ["class"],
  ...,
	theme: {
		extend: {
			 flex: {
        '2': '2 2 0%',
        '3': '3 3 0%',
        '4': '4 4 0%',
        '5': '5 5 0%',
      },
      colors: {
        border: "var(--border)",
        input: "var(--input)",
        ring: "var(--ring)",
        foreground: "var(--foreground)",
        background: {
          DEFAULT: "var(--background)",
          on: "var(--background-on)",
          sub: "var(--background-sub)",
        },
        primary: {
          DEFAULT: "var(--primary)",
          on: "var(--primary-on)",
        },
        secondary: {
          DEFAULT: "var(--secondary)",
          on: "var(--secondary-on)",
        },
        ...,
      }
		},
	},
} satisfies Config

export default config
```

### className에 사용

```typescript
bg-card text-card-on
data-[state=active]:border-primary
data-[state=on]:text-card-on
flex-1 flex-2
```

이런 느낌의 테마가 됩니다.

![](https://i.imgur.com/Na1PBKY.png)
