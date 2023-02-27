---
layout: post
title:  Power Bi - Get users who have access to app (not workspace)
author: nickstugr
redirect_from: /2020/12/16/powerbi-get-users-who-have-access-to-app-not-workspace
---

Power Bi doesn't give you a way to get a list of users who you have granted access to an app. This is different to the list of users who have access to a workspace, which you _can_ get using the API.

Go to Power Bi and choose to update app, then go to the the permissions tab and you can see that the list just shows names, and you cannot export.

![app-permissions](/assets/posts/powerbi-app/app-permissions.png)

Open Chrome F12 dev tools, and inspect a users name and you can see that there is a title attribute that contains the User Principal Name which we are interested in.

![f12](/assets/posts/powerbi-app/f12.png)

In the console at the bottom, paste this command to return a semi-colon delimited list of UPNs:

```
$$("span[class=displayName]").forEach(user => { console.log(user.title + ";") })
```

![console](/assets/posts/powerbi-app/console.png)
