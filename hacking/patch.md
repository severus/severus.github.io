---
layout: default
title: Patches
parent: Hacking
nav_order: 1
---

# Creating and appying patches

The patch file (also called a patch for short) is a text file that consists of a list of differences between the original and updated file(s). Updating files with patch is often referred to as applying the patch or simply patching the files.

## How to create a patch file

### Using `git`:

```
cd program-directory
git add filechanges...
git commit (write a clear patch description)
git format-patch --stdout HEAD^ > toolname-patchname-YYYYMMDD-SHORTHASH.diff
```

### Using `diff`:

```
cd modified-program-directory/..
diff -up original-program-directory modified-program-directory > \
           toolname-patchname-RELEASE.diff
```

## How to apply a patch

### Using `git`:

```
cd program-directory
git apply path/to/patch.diff
```

### Using `patch`:

```
cd program-directory
patch -p1 < path/to/patch.diff
```

## References

* [patch (Unix) - Wikipedia](https://en.wikipedia.org/wiki/Patch_(Unix))
* [suckless.org software that sucks less](https://suckless.org/hacking/)
