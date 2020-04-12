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
- needs private key online
