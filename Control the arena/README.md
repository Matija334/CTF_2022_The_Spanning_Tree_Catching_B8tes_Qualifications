# CTF 2022 - Spanning Tree Qualifications: Control the arena

## Description
![desc](https://user-images.githubusercontent.com/111431985/197606538-063757e0-ce7e-4ad1-9fee-ca0a4bcccb72.png)

## Attached files
-

## Flag
```cyber_YouOwnTh3Arena!```

## Solution

In the web challenge, we first logged in to the website, where we were greeted with the message: "You don't have the right permissions. Greetings from President Snow"
After quick inspection, we found that this will be about JWT token. /admin peeked our interest since it was returning "401 Unauthorized". From the JWT token we could see
"admin" proprety was set to false. Our goal was to set it to true and access the /admin. 
```
{
  "alg": "RS256",
  "typ": "JWT"
}
{
  "iss": "seneca.crane@capitol.gov",
  "sub": "63480a8c60fe61a9dbf00c40",
  "iat": 1665684188966,
  "exp": 1665770588966,
  "admin": false
}
{
    <SIGNATURE STUFF REDACTED>
}
```

First we tried "none attack". This means we modified the JWT token and tried to access the /admin. Token looked like that: 
```
{
  "alg": "none",
  "typ": "JWT"
}
{
  "iss": "seneca.crane@capitol.gov",
  "sub": "63480a8c60fe61a9dbf00c40",
  "iat": 1665684188966,
  "exp": 1665770588966,
  "admin": true
}
```

After that didn't work, we tried algorithm confusion. We changed RS256 TO HS256 algorithm, which takes only secret phrase to sign - RSA public key in our case.
How did we get RSA public key?
Everytime we logged in, new JWT token was generated, so we wrote down multiple tokens, from which we recovered RSA public key.
Token looked like that:
```
{
  "alg": "HS256",
  "typ": "JWT"
}
{
  "iss": "seneca.crane@capitol.gov",
  "sub": "63480a8c60fe61a9dbf00c40",
  "iat": 1665684188966,
  "exp": 1665770588966,
  "admin": true
}
RSA public key
```
The attack was performed with burp suite, but unfortunatelly it didn't work.

Our last try was generating new pair of RSA key in burp suite, under "JWT Editor Keys" extention, change admin to true and sign it with new keys. We were successful!
![result](https://user-images.githubusercontent.com/111431985/197608520-4fcd838c-6292-4ff1-b94a-8961f046aa79.jpg)


