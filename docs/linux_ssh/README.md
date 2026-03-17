# `ssh`
- [Reference](#reference)
- [Description](#description)
- [Basic Usage](#basic-usage)
- [Common Options](#common-options)
- [`ssh-agent` and `ssh-add`](#ssh-agent-and-ssh-add)
- [Agent Forwarding](#agent-forwarding)
<hr>

### Reference
- [ssh(1) - Linux man page](https://linux.die.net/man/1/ssh)

<hr>

### Description
`ssh` command in Linux is used to securely connect to a remote server. Once connected, you can execute commands on the remote machine as if you were locally present.

<hr>

### Basic Usage
To initiate a secure connection, open you local terminal and use the following syntax:

```shell
ssh username@remote_host
```
- `username`: The username of your account on the remote server
- `remote_host`: The IP address or domain name of the remote server

If your local username is same as your remote username, you can simply use:

```shell
ssh remote_host
```

<hr>

### Common Options
You can modify the behavior of the `ssh` command using various flags
- `-p <port>`: Specifies a non-default port to connect to on the remote host. By default, SSH uses port 22.
    - Example: `ssh -p 2222 user@host`
- `-i <identity_file>`: Specifies the path to the private key file for public key authentication.
    - Example `ssh -i ~/.ssh/my_key user@host`
- `-v`: Enable verbose mode, helpful for troubleshooting connection problems
- `-C`: Requests compression of all data, useful on slower network connections
    - Example: `ssh -C user@host`
- `-J <user@jump-host>`: Connects to a targer server by first making an SSH conection to a jump host
    - Example: `ssh -J jumpuser@jump-server user@host-server`
- `D <local_port>`: Dynamic Port Forwarding — Turns your SSH client into SOCKS proxy server. Allows you to route all your local browser or app traffic through the remote server's IP.
    - Example: `ssh -D 8080 user@host`
- `L <local_port>:<remote_addr>:<remote_port>`: Local Forwarding — Forwards a port from your local machine to a port on the remote machine. Great for accessing remote databases like they are local.
    - Example: `ssh -L 5432:localhost:5432 user@host`

<hr>

### `ssh-agent` and `ssh-add`
`ssh-agent` is a background prcoess ("daemon") that runs on your local machine. Its sole job is to hold your decrypted private keys in memory.

When you try to SSH into a server, the `ssh` client talks to the agent to get the digital signature needed for the login, rather than asking you for a password.

The agent is usually started automatically. If not, you can manually run: 

```bash
eval $(ssh-agent)
```

`ssh-add` command is used to load your keys into the agent. A passphrase will be asked once to decrypt the key.

For the reset of your session, you can `ssh` into any number of servers without being prompted for that passphrase again.

```bash
# adds default keys (e.g. `id_rsa`) to the agent
ssh-add

# adds the specified key to the agaent
ssh-add ~/.ssh/id_rsa`
```

Some common options include:
- `-l`: Lists the fingerprints of all keys currently in the agent
- `-L`: Show the public versions of the keys in the agent
- `-d <file>`: Deletes a specific key from the agent
- `-D`: Deletes all keys from the agent

<hr>

### Agent Forwarding
Agent forwarding allows you to use your local SSH keys to authenticate on a remote server, securely passing credentials through an existing SSH connection. This enables access to servers without storing private keys on intermediate machines.

```bash
# after key is added to ssh-agent
ssh -A user@host

# run the following on the server to see local keys
ssh-add -L 
```

