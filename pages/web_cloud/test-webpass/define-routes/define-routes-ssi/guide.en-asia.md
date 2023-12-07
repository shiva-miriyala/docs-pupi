---
title: Server Side Includes (SSI)
updated: 2023-12-07
---

## Objective  

SSI commands enable you to include files within other pages.

At its most basic, you can include files within other ones so as not to repeat yourself.

Start by enabling SSI:


<!-- Web PaaS configuration-->
```yaml {configFile="routes"}
"https://{default}/":
    type: upstream
    upstream: "app:http"
    ssi:
        enabled: true
```

