---
title: "API Development Tips"
date: "2014-12-18"
tags: [node-js,software-architecture,web-services]
---

Organisations who are paying attention already know they need to have an open web API, and many already have under development or in the wild. Make sure you haven’t been caught by the pitfalls of many early API releases.

### Multiple points of failure

- Back-end systems: db servers/caches, hardware failures, etc.
- Interconnections: router failures, bad cables, etc.
- External Dependencies: fail whales, random cloud latency, etc.

### The 5 tips

1. **Test it all**
    - Unit test are not enough, they are just the beginning.
    - Test what users experience. Perform end-to-end black box tests.
    - Replay your access logs. Very accurate.
    - Validate return payloads. A stack trace is not valid XML.
2. **Plan for future versions**
    - Versions are not sexy/semantic (but do it anyway).
    - Announce versions often.
3. **Embrace standards**
    - APIs are better when predictable.
    - Standard approaches mean tools.
    - Avoid uncomfortable migrations. No one wants an OAuthpocalypse.
4. **Monitor everything & be honest**
    - Trends are your friend.
    - Users are not your early-warning ops team.
    - Be open and honest, or your users will tweet that your API sucks!
5. **Fail well**
    - Well-formed errors win friends and makes users more tolerant to failure.
    - Make monitoring easy.
    - Don't punish everyone. Determine who gets hurt most by failures.

Watch the video here: [Understanding API Activity by Clay Loveless](http://www.youtube.com/watch?v=_42d8lBMnVo&list=PL8EE11ACD3514A9EA&index=1)
