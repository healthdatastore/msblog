---
title: "How to establish SSH tunelling between host and remote server"
date: 2023-01-28T11:36:01+03:00
draft: false
---
### Local TCP forwarding

>***Problem: you need to connect from your local machine to postgres db hosted remotely (for example on VPS). But direct remote connections to database are not allowed***

![local tcp forwarding example](/ssh_local.png)

Let's assume we have a remote server named "host 2" with application (PostgreSQL for instance) which accepts incoming connections on port 5432. Instead of connecting to port 5432 remotely (which is insecure and might be blocked by the host's firewall) we can connect to port 32 allowed for remote connections by host's 2 firewall.

Thus, we need to connect from our app client located on our local machine (i.e. host 1) to the app server located on host 2.

To do this we can type in host's 1 terminal next command:

```
ssh -L 9999:localhost:5432 host2
```

Now we can connect from our local machine to remote app. For example, for PostgreSQL it could look like:

```
psql -h localhost -p 9999 -U postgres
```
