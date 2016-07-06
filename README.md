# centosmin

An alternative base Docker image, targeted particularly for people who
are statically compiling native code (Go, Rust) or minimal C/C++
servers (nginx, redis) that don't tend to have big dependencies and
don't need translation infrastructure, etc.

Implementation and size
-----------------------

Right now we're at 77MB.  Compared to the main base image, there are
only two packages we include directly: `centos-release` and [micro-yuminst](https://gitlab.com/cgwalters/micro-yuminst).
which is a trivial implementation of `yum -y install` in C.  Hence
this image doesn't include Python.

Everything else gets pulled in indirectly mostly via chains like:
  libhif -> libcurl -> libssh2 
         -> glib2 -> shared-mime-info

Approaching ~60MB by carefully pruning unused binaries and
documentation is likely possible.  After that, things get harder -
we'd likely need to e.g. have a separate libcurl build that only did
http and didn't drag in `libssh2` for example.

