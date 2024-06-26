### File and directory utility functions

```go
package main

import (
	"errors"
	"fmt"
	"os"
)

func FileExists(path string) bool {
	if _, err := os.Stat(path); errors.Is(err, os.ErrNotExist) {
		return false
	}
	return true
}

func DirExists(path string) bool {
	if _, err := os.Stat(path); errors.Is(err, os.ErrNotExist) {
		return false
	}
	return true
}

func CreateFileIfNotExists(path string) error {
	if _, err := os.Stat(path); os.IsNotExist(err) {
		file, err := os.Create(path)
		if err != nil {
			return err
		}
		defer file.Close()
	}
	return nil
}

func CreateDirIfNotExists(path string) error {
	err := os.Mkdir(path, 0777)
	if err == nil {
		return nil
	}

	if os.IsExist(err) {
		stat, err := os.Stat(path)
		if err != nil {
			return fmt.Errorf("os.Stat: failed to read %s: %v", path, err)
		}

		if !stat.IsDir() {
			return fmt.Errorf("path %s exists but is not a directory", path)
		}
		return nil
	}

	return err
}

// IsSymlink returns true if path is a hard symlink.
// https://stackoverflow.com/a/31889712
func IsSymlink(path string) (isSymlink bool, actualPath string) {
	// 'os.Lstat()' reads the link itself.
	// 'os.Stat()' would read the link's target.
	fi, err := os.Lstat(path)
	if err != nil {
		return false, ""
	}

	// True if the file is a symlink.
	if fi.Mode()&os.ModeSymlink != 0 {
		link, err := os.Readlink(path)
		if err != nil {
			return false, ""
		}
		return true, link
	}

	return false, ""
}
```

### Filepath

```go
package main

import (
	"fmt"
	"path/filepath"
)

func main() {
	path := "/Users/eaardal/.anyver/config.yaml"
	fmt.Printf("path: %s\n", path)
	// /Users/eaardal/.anyver/config.yaml

	abs, _ := filepath.Abs(path)
	fmt.Printf("filepath.Abs: %s\n", abs)
	// /Users/eaardal/.anyver/config.yaml

	fmt.Printf("filepath.Base: %s\n", filepath.Base(path))
	// config.yaml

	fmt.Printf("filepath.Clean: %s\n", filepath.Clean(path))
	// /Users/eaardal/.anyver/config.yaml

	fmt.Printf("filepath.Dir: %s\n", filepath.Dir(path))
	// /Users/eaardal/.anyver

	fmt.Printf("filepath.Ext: %s\n", filepath.Ext(path))
	// .yaml

	fmt.Printf("filepath.FromSlash: %s\n", filepath.FromSlash(path))
	// /Users/eaardal/.anyver/config.yaml

	fmt.Printf("filepath.IsAbs: %T\n", filepath.IsAbs(path))
	// true
}
```
