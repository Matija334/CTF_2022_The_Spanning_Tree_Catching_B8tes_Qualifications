# CTF 2022 - Spanning Tree Qualifications: Small is the new big

## Description

## Attached files
- flag2.pgp

## Flag
```cyber_Rs4_W1en3r_sMaLl_Pr1v_Exp0neNt```

## Solution
File flag2.pgp was found in the solution of the previous challenge (sniff). In the mounted from the sniff challenge we had to look for the public keys:
```GNUPGHOME=/mnt/.gnupg/ gpg -k``` From there we noticed Michael J. Wiener was the owner of the key. Next we exported his public key.
```GNUPGHOME=/mnt/.gnupg/ gpg --armor --export 0xD9497DFF522A6EB7 > ~/0xD9497DFF522A6EB7.asc```
Then we wrote script to extract e and n from the key.
```
import cryptography
from pgpy import PGPKey
from Crypto.PublicKey import RSA
pubkey, _ = PGPKey.from_file("public.key")
_, enckey = pubkey.subkeys.popitem()
km = enckey._key.keymaterial
e = int(km.e)
n = int(km.n)
print(e)
print(n)
```

With Wiener attack we were able to extract d and generate private key. We used following link https://github.com/pablocelayes/rsa-wiener-attack
```
private_key = RSA.construct((n, e, d))
privateKeyPem = private_key.exportKey(format='PEM', passphrase=None, pkcs=1, protection=None, randfunc=None)
print(privateKeyPem)
```

Next we had to import this private key
```GNUPGHOME=/mnt/.gnupg/ pem2openpgp "Michael J. Wiener" < privatekey.pem | GNUPGHOME=/mnt/.gnupg/ gpg --import```
and we were able to decode the file and read the flag.
```GNUPGHOME=/mnt/.gnupg/ gpg -d flag2.gpg```
