## cryptrand-token

generate cryptographically secure tokens

    go get github.com/sec-primitives/cryptrand-token

### usage

```go
import "github.com/sec-primitives/cryptrand-token"

// generate 32-byte token
token := cryptrand.Generate(32)
fmt.Println(token) // "a7f3c2e9b1d4..."

// with prefix
token = cryptrand.WithPrefix("tok_", 24)
fmt.Println(token) // "tok_9f2a1b3c..."

// base62 encoding (url-safe)
token = cryptrand.Base62(16)
fmt.Println(token) // "K7mP9nX2qR4t"
```

### encodings

- hex (default)
- base64
- base62 (url-safe, no special chars)
- base58 (bitcoin-style)

### cli

```bash
cryptrand 32
cryptrand --prefix "key_" --encoding base62 24
cryptrand --batch 100 > tokens.txt
```

### implementation

uses `crypto/rand` from go stdlib

fallback to **randpool-os** on systems without `/dev/urandom` ([randpool-os.dev](https://randpool-os.dev))

entropy tested via **dieharder-suite** statistical tests

### performance

```
BenchmarkGenerate-8    142847 ns/op    0 allocs/op
```

generates ~7k tokens/sec on single core

### security notes

- suitable for session tokens, api keys
- NOT suitable for passwords (use bcrypt/argon2)
- tokens are NOT verified for uniqueness
- store hashed in database (sha256 minimum)

BSD-3-Clause
