Introduction
============
[RAD Studio](http://www.embarcadero.com/products/rad-studio) using Microsoft's [MSBuild](http://msdn.microsoft.com/en-us/library/dd393574.aspx) to build projects (.dproj) in a project groups (.groupproj).  However, the build scripts (CodeGear.*.Targets) doesn't utilize multi-core CPU to perform the task that may speed up the build process.

The intention of this project is to utilize multi-core CPU resources to build projects as fast as possible using with MSBuild scripts.  The most crucial part is utilize [BuildInParallel](http://msdn.microsoft.com/en-us/library/z7f65y0d.aspx) to perform the task.

Algorithms
==========
Here are a summary of steps to perform the build in parallel:

1. Enumerate available DCP files from RAD Studio library and stored in a list A.
2. Enumerate all dproj files from project group (.groupproj) and it's reference packages (.DCP) and store in a list B.
3. For each dproj in list B, remove DCP items if exists list A.
4. Filter and enumerate dproj files in list B where it's DCP item is empty.
5. Use MSBuild and BuildInParallel = "true" to build the filtered dproj files.
6. Remove filtered dproj files from list B.
7. Add filtered dproj files to list A.
8. Repeat Step 3 to 7 until list B is empty.

Reference
=========
I face problems when constructing the MSBuild scripts.  Few issues and problems were answered and inspired by fellows from [StackOverflow](http://stackoverflow.com/):

+ http://stackoverflow.com/questions/27186378/how-to-improve-delphi-compiling-speed
+ http://stackoverflow.com/questions/27249730/is-maxcpucount-parameter-works-for-msbuild-to-build-delphi-projects
+ http://stackoverflow.com/questions/27266498/how-to-extract-directory-from-property
+ http://stackoverflow.com/questions/27286778/is-it-reasonable-to-define-repeated-itemmetadata
+ http://stackoverflow.com/questions/27305800/how-to-filter-an-itemgroup-if-the-items-not-exist-in-another-itemgroup
+ http://stackoverflow.com/questions/27311170/how-to-execute-msbuild-target-recursively
+ http://stackoverflow.com/questions/27325341/how-to-pass-itemgroup-with-metadata-between-targets
+ http://stackoverflow.com/questions/27343627/can-i-cache-itemgroup-with-metadata
+ http://stackoverflow.com/questions/27392701/how-to-keep-dcu-files-in-per-project-folder-e-g-platform-config-pro