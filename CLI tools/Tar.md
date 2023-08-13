### Basics

```
TAR

c - Create an archive
x - Extract an archive
v - Verbose output, progress while archiving
f - File name of the archive file
z - Compressed gzip archive
C - Path to save the output
t - View content of archive

Archive path in the current directory
tar -cvf {file}.tar /path/to/archive

Compress archive with -z option
tar -cvzf {file}.tar.gz (or .tgz) /path/to/archive

Untar/extract with -x option (same for .tar.gz)
tar -xvf /path/to/tar

Untar/extract to directory (same for .tar.gz)
tar -xvf /path/to/tar -C /path/to/save

List content of tar with -t option
tar -tvf {file}.tar
```
