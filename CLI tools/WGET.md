### Basics

```
WGET

Download and unpack to current working directory.
The wget option -O specifies a file to which the documents is written, and here we use -, meaning it will written to standard output and piped to tar and the tar flag -x enables extraction of archive files and -z decompresses, compressed archive files created by gzip.

wget -c {url}.tar.gz -O - | tar -xz

Extract to custom directory
wget -c {url}.tar.gz -O - | sudo tar -xz -C /path/to/extract/

Download file to disk and then extract it
wget -c {url}.tar.gz && tar -xzf {filename}.tar.gz

Same as above but extract to custom directory
wget -c {url}.tar.gz && tar -xzf {filename}.tar.gz -C /path/to/extract/
```
