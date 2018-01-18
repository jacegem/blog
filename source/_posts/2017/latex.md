---
title: LATEX
date: 2017-02-18
tags: [LATEX]
categories:
- Programming
- ETC
---

# LATEX

설치

TexStudio

Beamer
```tex
\documentclass{beamer}

\usepackage[utf8]{inputenc}
\usepackage[hangul]{kotex}

%Information to be included in the title page:
\title{Sample title 한글}
\author{Anonymous}
\institute{ShareLaTeX}
\date{2014}



\begin{document}

\frame{\titlepage}

\begin{frame}
\frametitle{Sample frame title}
This is a text in first frame. This is a text in first frame. This is a text in first frame.
\end{frame}

\end{document}
```

한글 사용시 `\usepackage[hangul]{kotex}` 패키지를 추가한다.
