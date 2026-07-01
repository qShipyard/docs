# Source: https://github.com/qShipyard/narratorlog/blob/main/deploy/nginx.conf

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

 

## FilesExpand file tree

 main

/

# nginx.conf

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

[![phonixcode](https://avatars.githubusercontent.com/u/48732133?v=4&size=40)](https://github.com/phonixcode) [phonixcode](https://github.com/qShipyard/narratorlog/commits?author=phonixcode)

[deployment setup](https://github.com/qShipyard/narratorlog/commit/45ffed557ae000275864fe157839db14871638fe)

failure

Jun 26, 2026

[45ffed5](https://github.com/qShipyard/narratorlog/commit/45ffed557ae000275864fe157839db14871638fe) · Jun 26, 2026

## History

[History](https://github.com/qShipyard/narratorlog/commits/main/deploy/nginx.conf)

Open commit details

History

68 lines (56 loc) · 1.51 KB

## FilesExpand file tree

 main

/

# nginx.conf

Copy path

Top

## File metadata and controls

- Code
 
- Blame
 

68 lines (56 loc) · 1.51 KB

[Raw](https://github.com/qShipyard/narratorlog/raw/refs/heads/main/deploy/nginx.conf)

Copy raw file

Download raw file

Open symbols panel

Edit and raw actions

events { worker\_connections 1024; } http { upstream api { server api:8080; } upstream web { server web:3000; } server { listen 80; server\_name \_; location /.well-known/acme-challenge/ { root /var/www/certbot; } location / { return 301 https://$host$request\_uri; } } server { listen 443 ssl; server\_name \_; ssl\_certificate /etc/letsencrypt/live/${DOMAIN}/fullchain.pem; ssl\_certificate\_key /etc/letsencrypt/live/${DOMAIN}/privkey.pem; ssl\_protocols TLSv1.2 TLSv1.3; ssl\_ciphers HIGH:!aNULL:!MD5; ssl\_prefer\_server\_ciphers on; client\_max\_body\_size 10M; # API and auth routes go to Go API location ~ ^/(api|auth|webhooks|health) { proxy\_pass http://api; proxy\_set\_header Host $host; proxy\_set\_header X-Real-IP $remote\_addr; proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for; proxy\_set\_header X-Forwarded-Proto $scheme; } # WebSocket for scan progress location /ws { proxy\_pass http://api; proxy\_http\_version 1.1; proxy\_set\_header Upgrade $http\_upgrade; proxy\_set\_header Connection "upgrade"; proxy\_set\_header Host $host; proxy\_read\_timeout 86400; } # Everything else goes to Next.js location / { proxy\_pass http://web; proxy\_set\_header Host $host; proxy\_set\_header X-Real-IP $remote\_addr; proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for; proxy\_set\_header X-Forwarded-Proto $scheme; } } }

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58

59

60

61

62

63

64

65

66

67

68

events {

worker\_connections 1024;

}

http {

upstream api {

server api:8080;

}

upstream web {

server web:3000;

}

server {

listen 80;

server\_name \_;

location /.well-known/acme-challenge/ {

root /var/www/certbot;

}

location / {

return 301 https://$host$request\_uri;

}

}

server {

listen 443 ssl;

server\_name \_;

ssl\_certificate /etc/letsencrypt/live/${DOMAIN}/fullchain.pem;

ssl\_certificate\_key /etc/letsencrypt/live/${DOMAIN}/privkey.pem;

ssl\_protocols TLSv1.2 TLSv1.3;

ssl\_ciphers HIGH:!aNULL:!MD5;

ssl\_prefer\_server\_ciphers on;

client\_max\_body\_size 10M;

\# API and auth routes go to Go API

location ~ ^/(api|auth|webhooks|health) {

proxy\_pass http://api;

proxy\_set\_header Host $host;

proxy\_set\_header X-Real-IP $remote\_addr;

proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for;

proxy\_set\_header X-Forwarded-Proto $scheme;

}

\# WebSocket for scan progress

location /ws {

proxy\_pass http://api;

proxy\_http\_version 1.1;

proxy\_set\_header Upgrade $http\_upgrade;

proxy\_set\_header Connection "upgrade";

proxy\_set\_header Host $host;

proxy\_read\_timeout 86400;

}

\# Everything else goes to Next.js

location / {

proxy\_pass http://web;

proxy\_set\_header Host $host;

proxy\_set\_header X-Real-IP $remote\_addr;

proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for;

proxy\_set\_header X-Forwarded-Proto $scheme;

}

}

}