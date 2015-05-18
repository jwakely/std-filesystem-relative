# Additions to Filesystem supporting Relative Paths

| Document Number: | DXXXX                      |
|:-----------------|:---------------------------|
| Date:            | 2015-05-12                 |
| Revises:         | None                       |
| Project:         | Programming Language C++   |
| Project number:  | TS 18822                   |
| Reply-to:        | ja11sop                    |

## Abstract

This paper proposes the addition of ...

## Table of Contents

  * [1. Introduction](#1-introduction)
  * [2. Motivation and Scope](#2-motivation-and-scope)
  * [3. Proposal and Design Discussion](#3-proposal-and-design-discussion)
  * [4. Proposed Wording](#4-proposed-wording)
  * [Acknowledgements](#acknowledgements)

## 1. Introduction

The [Filesystem TS - N3940](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n3940.html) introduces relative paths.

* They are defined in section **4.18 relative path [fs.def.relative-path]** 
* A decomposition method `relative_path()` is described in section **8.4.9 path decomposition [path.decompose]** 
* Two query methods to determine if a path either `has_relative_path()` or `is_relative()` described in **8.4.10 path query [path.query]**

However there is no way to create a relative path as a path relative to another. Methods are provided to create absolute and canonical paths.

In section **15.1 Absolute [fs.op.absolute]**:
 
```cpp
path absolute(const path& p, const path& base=current_path());
```

and in section **15.2 Canonical [fs.op.canonical]**

```cpp
path canonical(const path& p, const path& base = current_path());
path canonical(const path& p, error_code& ec);
path canonical(const path& p, const path& base, error_code& ec);
```

By providing operations to achieve absolute and canonical paths there is no impediment to providing a similar operation `relative()` that attempts to return a new path relative to some base path. 

## 2. Motivation and Scope

By providing operations to achieve absolute and canonical paths there is no impediment to providing a similar operation `relative()` that attempts to return a new path relative to some base path.

For example:


```cpp
path relative(const path& p, const path& to = current_path());
path relative(const path& p, error_code& ec);
path relative(const path& p, const path& to, error_code& ec);
```

This would return a `path`, if possible, that is relative to `to`. The implementation can make use of `absolute()` and `canonical()` to determine the relative path, if it exists.

The File System TS is based on the `​boost::filesystem` library and it too suffers from this anomaly. There are open tickets for this in ​Boost Trac:

  * [https://svn.boost.org/trac/boost/ticket/5897](#5897 Make path relative function)
  * [https://svn.boost.org/trac/boost/ticket/1976​](#1976 Inverse function for complete)

and it is the subject of several posts on StackOverflow for example:

  * ​[http://stackoverflow.com/questions/10167382/boostfilesystem-get-relative-path](http://stackoverflow.com/questions/10167382/boostfilesystem-get-relative-path)
  * [http://stackoverflow.com/questions/5772992/get-relative-path-from-two-absolute-paths](http://stackoverflow.com/questions/5772992/get-relative-path-from-two-absolute-paths)

Other languages typically provide a similar function. For example python provides:

> **`os.path.relpath(path[, start])`**
>
> Return a relative filepath to `path` either from the current directory or from an optional `start` directory. This is a path computation: the filesystem is not accessed to confirm the existence or nature of `path` or `start`.
> 
> `start` defaults to `os.curdir`

## 3. Proposal and Design Discussion

Proposal here...

## 4. Proposed Wording

Modify section:

**6 Header <filesystem> synopsis [fs.filesystem.synopsis]**

by adding the operational functions after `canonical`:

----
```cpp
path normalize(const path& p) noexcept;

path relative(const path& p, const path& to = current_path());
path relative(const path& p, error_code& ec);
path relative(const path& p, const path& to, error_code& ec);
```
----

Insert the section:

----
**15.3 Normalize [fs.op.normalize]**

```cpp
path normalize(const path& p) noexcept;
```

 * *Overview:*
    Return a normalized path of `p` by collapsing all redundant current "`.`" and parent "`..`" directory elements.

 * *Returns:*
    A normalized path which may be relative or absolute but it will not contain any current "`.`" directory element or any parent "`..`" directory elements after the first non-parent element.

**15.4 Relative [fs.op.relative]**

```cpp
path relative(const path& p, const path& start = current_path());
path relative(const path& p, error_code& ec);
path relative(const path& p, const path& start, error_code& ec);
```

  * *Overview:* Return a relative path of `p` to the current directory or from an optional `start` path.

  * *Returns:* A relative path, if the paths share a common 'root-name', otherwise `path()`. The relative path returned will satisfy the conditions shown in the following list. The `common` path is the common path that is shared between `p` and `start`. `rel_p` and `rel_start` are the divergent relative paths that remain after the `common` path is removed.
    * if `exists(start)`
      * if `exists(p)` then `equivalent(start/relative(p,start),p) == true`
      * else `normalize(canonical(start)/relative(p,start)) == canonical(common)/normalize(rel_p)`
    * else
      * if `exists(p)` then `normalize(canonical(common)/rel_start)/relative(p,start)) == canonical(p)`
      * else `normalize(start/relative(p,start)) == normalize(p)`

  * *Throws:* As specified in Error reporting.

  * *Remarks:* `exists(start) && !is_directory(start)` is an error.

----

and bump all following sections up by 0.2. Update the contents and any cross-references accordingly.

## Acknowledgements

Todo