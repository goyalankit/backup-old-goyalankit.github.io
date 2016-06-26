---
layout: post
title: "User Namespace in Red Hat Enterprise Linux 7.2"
date: 2016-06-25 15:34
comments: true
published: false
categories: 
---


Red Hat announced the availability of [user namespace](https://lwn.net/Articles/532593/) in rhel 7.2 [release notes](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html-single/7.2_Release_Notes/index.html#technology-preview-kernel), but they don't give details on how to use them. By default in rhel 7.2, user namespaces are disabled.

### Verify if user namespace is enabled
You can run a quick check by executing the [demo_userns.c](https://lwn.net/Articles/539941/) program, that creates a child in new user namespace. The child simply prints its effective user, groupd IDs and capabilities. If it runs successfuly, then namespaces are already enabled for you. However, if it returns something like `invalid arguments`, then user namespaces are disabled.


### Enable user namespace
To enable user namespace, you need to change one of the kernel parameters. You can do it by running following command:


```
grubby enable kernel namespace
```

Reboot the box.

Now you can verify by running demo_ns.c again and it should print user variables.
