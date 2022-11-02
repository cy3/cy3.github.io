---
layout: post
title: jekyll 에서 latex 사용하기
date: 2022-04-11 08:52:21 +0000
tags: cs latex 
---



## Latex test
$$
    f(x) = \int_{-\infty}^{\infty}
    \hat f(\xi)\,e^{2 \pi i \xi x}
    \,d\xi
$$

## troubleshooting
처음에 찾은 방법은 html script를 삽입하는 방법이였는데, 이상하게 local에서는 잘 뜨는데 github에 올리면 로딩이 안 되었다.   

[MathJax not rendering on .github.io · Issue #307](https://github.com/github/pages-gem/issues/307)를 찾아보니, https가 아닌 http 를 링크에 사용한 것이 원인이였다.    

아래 코드를 `head.html` 이나 포스팅에 포함되는 html 아무데나 넣어두면 잘 작동한다.

```html
  <script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
```

