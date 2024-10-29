# SSH Cheatsheet


![](https://github.com/abrahimcse/Linux-CLI-Cheatsheet/blob/main/Images/SSH%20CheatSheet.png)

Here's a comprehensive `SSH cheatsheet` with essential commands and options that cover basic usage, advanced techniques, and tips for managing SSH connections:


## Basic SSH Command

`1. SSH into a Remote Server`
```
ssh username@remote_host
```

- `username:` The user on the remote server.
- `remote_host:` The IP address or domain of the server.

`2. Specify a Port`

By default, `SSH` uses `port 22`. To connect on a different port:
```
ssh username@remote_host -p port_number
```

`3. Connect with an Identity File (Private Key)`
```
ssh -i /path/to/private_key username@remote_host
```
`4. Execute a Command on the Remote Server`
```
ssh username@remote_host "command_to_execute"
```
**Example:**
```
ssh user@server "ls /var/www"
```
`5. Enable Debugging Output`

To debug an SSH connection:

```
ssh -v username@remote_host
```
- Add more `-v` for increased `verbosity`: `-vv`, `-vvv`.

## Copying Files Using SCP (Secure Copy Protocol)

`6. Copy a File from Local to Remote`
```
scp /path/to/local_file username@remote_host:/path/to/remote_directory
```
`7. Copy a File from Remote to Local`
```
scp username@remote_host:/path/to/remote_file /path/to/local_directory
```
`8. Copy a Directory Recursively`
```
scp -r /path/to/local_directory username@remote_host:/path/to/remote_directory
```
## SSH Key Management
`9. Generate a New SSH Key Pair`
```
ssh-keygen -t rsa -b 4096 
```
`10. Copy Public Key to Remote Server (Passwordless SSH)`
```
ssh-copy-id username@remote_host
```
`11. Manually Copy Public Key to Server`
```
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
## SSH Config File

`12. Create/Edit SSH Config File`

You can simplify connections using the SSH config file at ~/.ssh/config:
```
nano ~/.ssh/config
```
**Example SSH Config Entry:**
```
Host myserver
  HostName 192.168.1.100
  User username
  IdentityFile ~/.ssh/id_rsa
  Port 22
```
Then, you can connect with:
```
ssh myserver
```
## Tunneling and Port Forwarding
`13. Local Port Forwarding (Access Remote Service Locally)`
```
ssh -L local_port:remote_host:remote_port username@remote_host
```
**Example: Access a remote MySQL database on `localhost:3306:`**
```
ssh -L 3306:localhost:3306 user@remote_host
```
`14. Remote Port Forwarding (Expose Local Service to Remote)`
```
ssh -R remote_port:localhost:local_port username@remote_host
```
**Example: Expose a local web server running on port `8080` to a remote server:**
```
ssh -R 8080:localhost:8080 user@remote_host
```
`15. Dynamic Port Forwarding (Create SOCKS Proxy)`
```
ssh -D local_port username@remote_host
```
**Example: Create a SOCKS proxy on `localhost:1080`:**
```
ssh -D 1080 user@remote_host
```
This SSH cheatsheet covers essential commands and features for managing SSH connections, file transfers, port forwarding, and automation



