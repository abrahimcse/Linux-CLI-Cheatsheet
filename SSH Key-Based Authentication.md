# SSH Key-Based Authentication

SSH Key-Based Authentication is a secure way to log into a server using a pair of cryptographic keys: a private key (kept secret on your device) and a public key (stored on the server). It enhances security by eliminating passwords, as the private key is never sent over the network. When you connect, the server verifies the public key, and if it matches, access is granted. This method is widely used for secure remote access.

`1.Generate SSH Key Pair:`

 ```
ssh-keygen -t rsa -b 4096 
```
`2.Copy Public Key to Remote Server:`
```
ssh-copy-id username@server_ip
```
`3.Connect Using SSH Key:`
```
ssh username@server_ip
```

**No more passwords! Key-based authentication makes your connection secure and seamless.**