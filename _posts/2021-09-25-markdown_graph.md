---
title: "markdown_graph"
date: 2021-09-25
categories: Basic_information
---


<div class="mermaid"> 
graph LR
A[Hard edge] -->B(Round edge)
    B --> C{Decision}
    C -->|One| D[Result one]
    C -->|Two| E[Result two]
</div>

<div class="mermaid"> 
  graph TD; A-->B; A-->C; B-->D; C-->D; 
</div>
