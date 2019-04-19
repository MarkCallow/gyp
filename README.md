GYP can Generate Your Projects.
===============================

This fork of the [upstream GIT repo](https://chromium.googlesource.com/external/gyp)
at Google has several additional features:

1. The msvs generator sets a property to prevent VS2010+ from attempting
to link into the application `*.obj` files that are sources for copies
or actions. [crbug 525](https://bugs.chromium.org/p/gyp/issues/detail?id=525).
See branch `crbug-525`.

2. The msvs generator supports Emscripten's [vs-tool](https://github.com/juj/vs-tool/)
plug-in for Visual Studio 2010. That is, it recognizes the properties
`vs-tool` offers to control the Emscripten compiler and linker. These
properties can be included in an `msvs_settings` dictionary. Note this
feature has only been tested with VS 2010. See branch `vs-tool_support`.

    All GYP msvs and ninja tests pass on Windows with these changes.
    
3. The make generator relativizes source paths of `copies` that contain,
but don't start with, environment variables, e.g $(BUILDTYPE). Standard
GYP only relativizes paths not containing environment variables. The
make generator also relativizes paths specified in `library_dirs`. See
branch `make_changes_copies` for the first and `crbug-512` for the second.

    All GYP make, ninja and cmake tests pass on Linux with these changes.

4. Fixes for
crbugs [512](https://bugs.chromium.org/p/gyp/issues/detail?id=512),
crbugs [513](https://bugs.chromium.org/p/gyp/issues/detail?id=513) and
crbugs [514](https://bugs.chromium.org/p/gyp/issues/detail?id=514).
See branch `crbug-NNN`.

5. A fix for crbug [550](https://bugs.chromium.org/p/gyp/issues/detail?id=550) in the Xcode generator.

6. The `make` generator has various changes to to better support
generating projects to build Android native applications. These
include additions to the `make_global_settings` dictionary such as
being able to create _simply expanded variables_ and to specify a
`TARGET_ABI`. The latter becomes a component of `builddir` in the
generated make files. It also in include changes to the handling of
path relativization in various cases in addition to those no. 3.
See branch `make_changes_all`.
 
    These changes are a work in progress and are known to break several
    of the GYP `make` generator tests.

Branch `remaster` contains 1, 2, 3, 4 & 5 as of this writing.

Branch `master` is identical with upstream/master, retained so `gclient`
tools can be used to upload changes to GYP's Gerrit for review.

No. 1 has been submitted as a patch in
[crbug 525](https://bugs.chromium.org/p/gyp/issues/detail?id=525).

No. 2 will not be submitted upstream as there are no plans for
`vs-tool` to support anything more modern than Visual Studio 2010.

No. 3 may be submitted in future.

No. 4 bug fixes have been submitted to Google.

No. 5 bug fix has been reported to Google and [PR #40](https://github.com/refack/GYP/pull/40) has been submitted to the GYP3 repo. on GitHub.

No. 6 is not yet complete.

The following feature originating from this fork has been merged
into upstream's master and the master branch of this repo.

+ The xcode generator recognizes the full set of build variables
that correspond to the set of destination names for the copy phase,
as appears in the Xcode UI.
See [the commit](https://chromium.googlesource.com/external/gyp/+/1f989f652a3017858c1e418c8344f04baee59f51)
for the changes.

### Installing GYP

GYP is a Python application. It is compatible with Python 2.7 and Python 3. 3.7+ is recommended.

To install, clone the repo to your machine and run the following
commands in a shell:

```bash
cd <your_gyp_clone>; sudo ./setup.py install
```

On Windows, use either a Git Bash or Cygwin Bash shell and omit the
`sudo`. Alternatively enter the following commands at a Windows Command
Prompt (`cmd.exe`):

```cmd
cd <your_gyp_clone>
python setup.py install
```

Documents are available at [gyp.gsrc.io](https://gyp.gsrc.io), or you
can check out the ```md-pages``` branch to read those documents offline.

### Installing Python

Visit the [Python Downloads](https://www.python.org/downloads/) page
to learn how to install Python for your OS.

On OS X you can use the Apple provided Python 2.7 which is in `/usr/bin`
or you can install a more recent version using the
[python.org](www.python.org) installer. If using the latter, and you want
to run the GYP tests or generate Ninja format projects on a Mac, you must
install [PyObjC](https://pythonhosted.org/pyobjc/),
which is included in the Apple-provided Python. The easiest way is

```bash
pip install -u pyobjc
```

On Windows you can use either the
[native Windows version](https://www.python.org/downloads/windows/)
(recommended) or the version found in [Cygwin](https://www.cygwin.com).
To run the GYP tests or generate Ninja format projects on Windows
you must install the [Python for Windows
extensions](https://sourceforge.net/projects/pywin32/?source=typ_redirect).

If you install the native Windows version you must add `<python>` and
`<python>/Scripts` to the PATH environment in Windows. `<python>` represents
your actual install directory which defaults to `C:\Python27`.
