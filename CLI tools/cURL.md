### Basics

```
CURL
-s: silent, -S: show error,

Methods
curl -X {VERB} {url}

Data
curl -X POST {url} -d '{data}'
curl -X POST -H "Content-Type: application/json" -d '{"foo": "bar"}' {url}
curl -X POST -H "Content-Type: application/json" -d @./file.json {url}

Return HTTP headers only
curl -I {url}

Save result
curl -o {name} {file url}
curl -O {file url} (saves with same filename)

Add header
curl -H "{header}: {value}" {url}

Archived files
Download and unpack to current working directory
curl {url}.tar.gz | tar -xz

Extract to custom directory
curl {url}.tar.gz | tar -xz  -C /path/to/extract/

Same as above but differnet syntax
curl {url}.tar.gz && tar -xzf {filename}.tar.gz -C /path/to/extract/
```
