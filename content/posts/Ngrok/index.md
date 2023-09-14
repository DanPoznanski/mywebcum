---
title: "Ngrok"
discription: ngrok 
date: 2023-09-03T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","Proxy"]
showTableOfContents: true
--- 

## Ngrok on Ubuntu
sign up on ngrok website

after download ngrok Unzip 

create folders and file

root/.config/ngrok/ngrok.yml
```yml
# in ngrok.yml
authtoken: 2UoPmLJDDOzSLlTA3obRZuswHTN_ZbmHcjZYLDdsPShY5KYP
version: 2
region: us
```
for config 
`./ngrok config edit`



to run
`./ngrok http 8080 `

