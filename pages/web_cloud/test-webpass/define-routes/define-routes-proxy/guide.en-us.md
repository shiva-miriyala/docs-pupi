---
title: Proxy routes
updated: 2023-12-07
---

> [!primary]  
> 
> Only use this feature to address edge cases where you need to proxy to another, outside project.</br>
> **Do not use this for internal routing.**</br>
> To expose your app to the outside world, see [how to define routes](../define-routes-define-routes).
> 
> 
​
Sometimes you want your app to pass requests on to a different Web PaaS project.
Basic redirects only work within the same project, so use proxy routes for routes elsewhere.​

You can define an external proxy on your Web PaaS project by defining a route like the following:


<!-- Web PaaS configuration-->
```yaml {configFile="routes"}
https://{default}/foo:
    type: proxy
    to: https://www.example.com
```

