---
layout: post
title: How to update expired repository keys in Debian / Ubuntu
category: posts
---

When using third party package repositories, you occassionally might need to
update expired repository keys. An expired key leads to an error message
during a `sudo apt-get update` similar to the following:

    W: An error occurred during the signature verification. The repository is not updated and the previous index files will be used.
    GPG error: http://build.i3wm.org raring Release:
    The following signatures were invalid: KEYEXPIRED 1396011159 KEYEXPIRED 1396011159 KEYEXPIRED 1396011159

To find any expired repository keys and their IDs, use `apt-key` as follows:

    apt-key list | grep expired

You will get a result similar to the following:

    pub   4096R/BE1DB1F1 2011-03-29 [expired: 2014-03-28]

The key ID is the bit after the `/` i.e. `BE1DB1F1` in this case.

To update the key, run

    sudo apt-key adv --recv-keys --keyserver keys.gnupg.net BE1DB1F1

The repository will then be updated with the next `sudo apt-get update`.
