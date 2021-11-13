---
title: "how to use markdown"
date: 2021-09-25
categories: basic-information
---

 ## 마크다운 작성법 (링크)  

<https://gist.github.com/ihoneymon/652be052a0727ad59601#file-how-to-write-by-markdown-md>  

<script src="https://gist.github.com/ihoneymon/652be052a0727ad59601.js"></script>


 ## LaTex 작성법 (링크)  

 <https://velog.io/@d2h10s/LaTex-Markdown-수식-작성법>

##  mermaid

<div class="mermaid"> 
graph LR;
    A[index.md] -->B[_layouts/home.html];
    B --> C[_includes/head.html];
    B --> D[_includes/toc-date.html];
    B --> E[_includes/body.html];
    B --> F[_includes/footer.html];
    E --> G[content];
</div>

<div class="mermaid"> 
  graph TD; A-->B; A-->C; B-->D; C-->D; 
</div>
