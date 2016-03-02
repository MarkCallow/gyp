GYP can Generate Your Projects.
===================================

This fork of the [upstream GIT repo](https://chromium.googlesource.com/external/gyp)
at Google has 3 additional features:

1. The xcode generator recognizes the full set of build variables
that correspond to the set of destination names for the copy phase,
as appears in the Xcode UI. Do
 ```
 git diff upstream_master xcode_changes
 ```
 to see the list. The original version only understands
 the variable `$(BUILT_PRODUCTS_DIR)` which corresponds to
 'Products Directory'. See branch `xcode_changes`.

    All GYP xcode tests pass with these changes.

2. The msvs generator supports Emscripten's [vs-tool](https://github.com/juj/vs-tool/)
plug-in for Visual Studio 2010. That is, it recognizes the properties `vs-tool` offers to
control the Emscripten compiler and linker. These properties
can be included in an `msvs_settings` dictionary. Note this feature
has only been tested with VS 2010. See branch `vs-tool_support`.

    All GYP msvs tests are expected to still pass. That will be tested soon.
    
3. Various changes to the `make` generator to better support generating
projects to build Android native applications. These include additions
to the `make_global_settings` dictionary such as being able to create
 _simply expanded variables_ and to specify a `TARGET_ABI`. The latter
 becomes a component of `builddir` in the generated make files. They
 also in include changes to the handling of path relativization in
 various cases. See branch `make_changes`.
 
    These changes are a work in progress and are known to break several
    of the GYP `make` generator tests.

Branch `master` contains 1 and 2 as of this writing.

Number 1 has been submitted to Google but before merging
they want tests, that I have not had time to write. The
same lack of tests prevents submission of 2 and 3.

GYP is a Python application requiring Python 2.7.

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

### Installing Python 2.7

Visit the [Python Downloads](https://www.python.org/downloads/) page
to learn how to install Python for your OS.

On OS X you can use the Apple provided Python 2.7 which is in `/usr/bin`
or you can install a slightly more recent version using the [python.org](www.python.org)
installer. If using the latter, and you want to run the GYP tests you
must install [PyObjC](https://pythonhosted.org/pyobjc/), which is
included in the Apple-provided Python. The easiest way is

```bash
pip install -u pyobjc
```

On Windows you can use either the [native Windows version](https://www.python.org/downloads/windows/)
(recommended) or the version found in [Cygwin](https://www.cygwin.com).

If you install the native Windows version you must add `<python>` and
`<python>/Scripts` to the PATH environment in Windows. `<python>` represents
your actual install directory which defaults to `C:\Python27`.
