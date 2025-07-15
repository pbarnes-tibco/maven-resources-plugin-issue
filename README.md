# maven-resources-plugin-issue
Reproduce maven-resources-plugin issue with file name filtering and file separator

Issue is that if you copy resources with the maven-resources-plugin copy-resources goal
on Windows, with configuration including `fileNameFiltering=true`, `escapeString=\`, and
resource files whose names begin with a maven property within a subfolder, then file name
filtering does not work. More generally, this fails on any platform if the escape string
is the same as the file separator.

For example, if you have a resource file under a resource directory with path
`folder\${file.name}.txt`, then the file is copied to the output directory as
a file name `folder${file.name}.txt`. Filtering is not done, and the location of
the file under `folder` is lost.

The filtering logic appears to be applied to the entire relative path name
`folder\${file.name}.txt`, and the file separator is treated as an escape.
