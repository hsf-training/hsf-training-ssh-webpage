# Reference

## Quick Command Reference

### Basic SSH Commands
```bash
# Connect to a server
ssh username@hostname

# Connect with verbose output (for debugging)
ssh -v username@hostname

# Copy file to remote server
scp localfile username@hostname:/remote/path/

# Copy file from remote server
scp username@hostname:/remote/path/file localfile

# Copy directory recursively
scp -r localdirectory username@hostname:/remote/path/
```

### SSH Key Management
```bash
# Generate a new SSH key pair
ssh-keygen -C "Comment for your key"

# Copy public key to server
ssh-copy-id -i ~/.ssh/id_rsa username@hostname

# Start ssh-agent
eval "$(ssh-agent -s)"

# Add key to ssh-agent
ssh-add ~/.ssh/id_rsa

# List keys in ssh-agent
ssh-add -l

# Remove host from known_hosts
ssh-keygen -R hostname
```

### Port Forwarding
```bash
# Local port forwarding
ssh -L localport:remotehost:remoteport username@hostname

# Example: Access remote Jupyter notebook
ssh -L 8080:localhost:8080 username@hostname
```

### Useful Tools
```bash
# Synchronize directories with rsync
rsync -vaz source/ destination/

# Start tmux session
tmux

# List tmux sessions
tmux ls

# Attach to tmux session
tmux attach

# Detach from tmux
Ctrl-b d
```

## Configuration File Location

SSH configuration file: `~/.ssh/config`

Authorized keys file: `~/.ssh/authorized_keys`

Known hosts file: `~/.ssh/known_hosts`

## Common SSH Config Options

```
Host nickname
    HostName full.hostname.com
    User yourusername
    Port 22
    IdentityFile ~/.ssh/id_rsa
    IdentitiesOnly yes
    ForwardAgent yes
    ProxyJump gateway.host.com
    ServerAliveInterval 60
    Compression yes
```

## Resources

- [OpenSSH Documentation](https://www.openssh.com/)
- [SSH Config Manual](https://linux.die.net/man/5/ssh_config)
- [Tmux Documentation](https://github.com/tmux/tmux/wiki)
- [RSA Algorithm](https://en.wikipedia.org/wiki/RSA_(cryptosystem))
