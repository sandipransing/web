---
layout: post
title: "Setting up nginx maximum upload size"
date: 2011-03-31T15:56:00+05:30
comments: false
categories:
 - nginx
---

Edit nginx configuration and look for `http` block inside it.

Then inside `htttp` block add following lines.

```
http {
    include conf/mime.types;
    default_type application/octet-stream;
    client_max_body_size 10m;
    ....
}
```

In above configuration *application/octet-stream* supports any kind of file upload.
