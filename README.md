# mentalpoker

Dealerless card shuffle ("mental poker") for two untrusting peers: a
commutative cipher lets both players jointly shuffle and deal a deck so that
neither player — nor any relay between them — learns the deck order or the
other's hand, yet every card can be revealed and verified at showdown.

Scheme: Pohlig–Hellman / SRA over a 2048-bit safe prime (RFC 3526 group 14).
Each card is a distinct quadratic-residue token; a player's secret is an
exponent, encryption is modular exponentiation, and because exponentiation
commutes the players can layer and strip encryption in either order. Cheating
is caught at showdown: both reveal their exponents and anyone (including
spectators) recomputes the shuffle and verifies the deck.

```go
key, _ := mentalpoker.NewKey()
deck := mentalpoker.EncryptAll(key, mentalpoker.FreshDeck())
_ = mentalpoker.Shuffle(deck) // crypto/rand Fisher–Yates
// exchange decks, strip layers with key.Decrypt, verify with VerifyDeck
```

Pure logic — no networking, no I/O. Compiles to WASM. Extracted from
[kibitz](https://github.com/richardwooding/kibitz), where it powers Gin Rummy
over an end-to-end-encrypted relay.

MIT licensed.
