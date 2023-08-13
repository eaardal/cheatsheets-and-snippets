### Regarding echo, IFS, line breaks

https://stackoverflow.com/a/22101842

```
$ f="fafafda
> adffd
> adfadf
> adfafd
> afd"
```

```
$ echo $f
fafafda adffd adfadf adfafd afd
```

```
$ echo "$f"
fafafda
adffd
adfadf
adfafd
afd
```

Without quotes, the shell replaces `$TEMP` with the characters it contains (one of which is a newline). Then, before invoking `echo` shell splits that string into multiple arguments using the `Internal Field Separator` (IFS), and passes that resulting list of arguments to `echo`. By default, the `IFS` is set to whitespace (spaces, tabs, and newlines), so the shell chops your `$TEMP` string into arguments and it never gets to see the newline, because the shell considers it a separator, just like a space.
