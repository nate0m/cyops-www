# A Guide to PAM

## What is PAM?

### What can I use it for?

## Finding out if a program is PAM-aware

Since programs usually communicate with PAM using its library, you can find out if a program uses PAM by checking if that library is compiled.

```sh
[~] ldd $(which login) | grep -i pam
    libpam.so.0 => /lib64/libpam.so.0 (0x00007f1672990000)
    libpam_misc.so.0 => /lib64/libpam_misc.so.0 (0x00007f167298b000)
```
