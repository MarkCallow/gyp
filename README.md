GYP can Generate Your Projects.

This fork of the upstream GIT repo at Google has 2 additional features:

1. The xcode generator recognizes the full set of build variables
that correspond to the set of destination names for the copy phase,
as appears in the Xcode UI. Do
```
git diff upstream_master xcode_changes
```
to see the list. The original version only understands
'Products Directory' which corresponds to the variable
`$(BUILD_PRODUCTS_DIR)`.

    All GYP xcode tests that pass on upstream_master pass with these
    changes. (There are 5 tests that fail in both cases.)

2. The msvs generator supports the Emscripten vs-tool plug-in.
That is, it recognizes the properties vs-tool offers to
control the Emscripten compiler and linker. These properties
can be included in an `msvs_settings` dictionary. Note this feature
has only been tested with Visual Studio 2010.

    All GYP msvs tests are expected to still pass. That will be tested soon.

The first of these has been submitted to Google but they want
tests, that I have not had time to write, before merging. The
same lack of tests prevents submission of the second.

* [User documentation](/docs/UserDocumentation.md)
* [Input Format Reference](/docs/InputFormatReference.md)
* [Language specification](/docs/LanguageSpecification.md)
* [GYP Hacking](/docs/Hacking.md)
* [GYP Testing](/docs/Testing.md)
* [GYP vs. CMake](/docs/GypVsCMake.md)
* [Source](/docs/Source.md)
* [Buildbot](/docs/Buildbot.md)
