# Stealth Addresses

The following sketches a new scheme for stealth addresses in bitcoin. This scheme is more compact because it requires only a single standard transaction which is indistinguishable from regular transactions. 

## Protocol 

### Sending
Alice wants to send Bitcoins to Bob. 

```
secret_alice  = a  public_alice = A = a * G 
secret_bob    = b    public_bob = B = b * G 
```
Bob has published `B` on a website.

Alice uses ECDH do generate a secret shared nonce `S`:
```
S = a * B 
```
Alice maps the point `S` to a scalar `s` (e.g. via Hashing or HMAC?)
```
s = Hash(S)
```
Alice creates a stealth key for Bob:
```
C = s * B
```
Alice sends Bitcoins from her key `A` to the stealth key `C`. 

(Side note: Actually the bitcoins are sent to the corresponding *address*)

### Receiving
Bob scans every transaction in the blockchain. 
```
For every transaction: 
  Let A be the sender's public key
  S = b * A
  s = Hash(S)
  C = s * B
  if C is the recipient's key
    => Bob received Bitcoins from A
```


## Limitations 
- Bob has to scan every transaction in the blockchain 
- For every transaction Bob has to perform two EC multiplications
- It's pretty ugly that Bob needs to extract the public key from the spending transaction though (what if it's a multisig, or a coinjoin, or some new script they don't understand?) 
- Bob needs private key online

## Improvements 

### Bob needs private key online
When the recipient Bob scanns the blockchain he needs his private key `b` to compute the shared nonce `S`. 
We can introduce a second private key such that we have separate keys for nonce generation and spending.
```
B_nonce = b_nonce * G
B_spend = b_spend * G
```
The private key `b_spend` can be kept offline while scanning. It is only needed to spend funds.

#### Improved Protocol 
Bob has published `B_nonce` and `B_spend` on a website.

Alice uses ECDH do generate a secret shared nonce `S`:
```
S = a * B_nonce 
```
Alice maps the point `S` to a scalar `s` (e.g. via Hashing or HMAC?)
```
s = Hash(S)
```
Alice creates a stealth key for Bob:
```
C = s * B_spend
```

Bob scans every transaction in the blockchain: 
```
For every transaction: 
  Let A be the sender's public key
  S = b_nonce * A
  s = Hash(S)
  C = s * B_spend
  if C is the recipient's key
    => Bob received Bitcoins from A
```
This requires only `b_nonce` and not `b_spend`.
