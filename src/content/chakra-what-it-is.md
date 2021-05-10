---
layout: post
title: 'What is Chakra?'
author: [Dan]
tags: ['framework']
image: img/welcome-to-ghost.jpg
date: '2021-05-10'
draft: false
excerpt: 챠크라 UI?
---

`ReactJS`를 위한 UI 라이브러리이다.

> [https://github.com/chakra-ui/chakra-ui](https://github.com/chakra-ui/chakra-ui)

```bash
npm i @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^4
# or 
yarn add @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^4
```

```jsx
import * as React from "react"
// 1. import `ChakraProvider` component
import { ChakraProvider } from "@chakra-ui/react"
function App() {
  // 2. Use at the root of your app
  return (
    <ChakraProvider>
      <App />
    </ChakraProvider>
  )
}
```