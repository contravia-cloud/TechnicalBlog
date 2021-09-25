---
title: "markdown_graph"
date: 2021-09-25
categories: Basic_information
---


<div class="mermaid"> 
graph LR;
    A[index.md] -->B[_layouts/home.html];
    B --> C[_includes/head.html];
    B --> D[_includes/toc-date.html];
    B --> E[_includes/body.html];
    B --> E[_includes/footer.html];
    E --> F[content];
</div>

<div class="mermaid"> 
  graph TD; A-->B; A-->C; B-->D; C-->D; 
</div>
