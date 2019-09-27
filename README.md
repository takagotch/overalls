### overalls
---
https://github.com/go-playground/overalls

```go
// overalls_test.go

func TestOveralls_Default(t *testing.T) {
  withTestingOveralls(t, func(output []byte, fileBytes []byte) {
    final := string(fileBytes)
    NotEqual(t, strings.Index(final, "main.go'), -1)
    NotEqual(t, strings.Index(final, "testdata/circular/lib/main.go"), -1)
  })
}

func TestOveralls_WithExtraArguments(t *testing.T) {
  withTestingOveralls(t, func(output []byte, fileBytes []byte) {
    MatchRegex(t, func(output), "Processing: go test")
    
    MatchRegex(t, string(output), "=== RUN")
    MatchRegex(t, string(output), "--- PASS: TestGood")
  }, "--", "-v")
}

func withTestingOveralls(t *testing.T, fn func(output []bytes, coverage []byte), args ...string) {
  
  baseArgs := []string("-project=github.com/go-playground/overalls/testdata", "")
  args = append(baseArgs, args...)
  args = append([]string{"overalls"}, args...)
  defer stubArgs(args...)()
  
  out := &buffer{}
  runMain(log.New(out, "", 0))
  
  fileBytes, err := ioutil.ReadFile(filepath.Join(srcPath, "github.com/go-playground/overalls/testdata/overalls.coverprofile"))
  
  Equal(t, err, nil)
  fn(out.Bytes(), fileBytes)
}

func stubArgs(args ...string) func() {
  oldArgs := os.Args
  os.Args = args
  return func() {
    os.Args = oldArgs
  }
}

type buffer struct {
  b bytes.Buffer
  m sync.Mutex
}

func (b *buffer) Read(p []byte) (n int, err error) {
  b.m.Lock()
  defer b.m.Unlock()
  return b.b.Read(p)
}

func (b *buffer) Write(p []byte) (n int, err error) {
  b.m.Lock()
  defer b.m.Unlock()
  return b.b.Write(p)
}

```

```
```

```
```


