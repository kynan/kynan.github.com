---
layout: post
title: Commands to replace the deprecated `apt-key` script
category: posts
---

`apt-key` is [deprecated](https://manpages.debian.org/testing/apt/apt-key.8.en.html),
however there is no replacement available, neither does the `man` page document
how to replace the commands `apt-key` provides. Here is my attempt.

**Note:** `gpg` will by default create new keyrings in the (new) "GPG keybox
database version 1", whereas `apt` expects them in the (legacy) "PGP/GPG key
public ring (v4)" format. Specify the prefix `gnupg-ring:` for the keyring file
to make `gpg` use the legacy v4 format.

## apt-key list

This commands lists all keys stored in `/etc/apt/trusted.gpg` and any `.gpg` or
`.asc` files in `/etc/apt/trusted.gpg.d`.

```shell
for f in /etc/apt/trusted.gpg /etc/apt/trusted.gpg.d/*.{asc,gpg}; do
  gpg --list-keys --keyid-format short --no-default-keyring --keyring $f
done
```

## apt-key adv

This command is used to download a key and store it in the "right" keyring.
`apt-key adv` merges all keyrings into one, downloads the new key(s) and then
merges back the changes. No need to replicate this setup.

### Updating an expired key

If you're updating an expired key, write it to the same keyring, replacing the
expired key. To find any keyrings containing an expired key, run the following:

```shell
for f in /etc/apt/trusted.gpg /etc/apt/trusted.gpg.d/*.{asc,gpg}; do
  $(gpg --list-keys --no-default-keyring --keyring $f | fgrep -iq expired) && echo "Expired key in $f"
done
```

Once you've identified the keyring and key ID, download the new key:

```shell
sudo gpg --recv-keys --no-default-keyring --keyring=gnupg-ring:/etc/apt/trusted.gpg.d/<FILENAME>.gpg --keyserver keys.gnupg.net <KEY_ID>
```

### Downloading a new key

When downloading a new key, create a new keyring in `/etc/apt/trusted.gpg.d`.
Note that on recent versions of `gpg`, this keyring will be in "GPG keybox
database version 1" format, which is incompatible with `apt-key`.

Choose a suitable filename for the new keyring and download the key:

```shell
sudo gpg --recv-keys --no-default-keyring --keyring=gnupg-ring:/etc/apt/trusted.gpg.d/<FILENAME>.kbx --keyserver keys.gnupg.net <KEY_ID>
```
