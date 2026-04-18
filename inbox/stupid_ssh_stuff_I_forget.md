# SSH Stuff

## basic ssh troubleshooting on wsl

if ssh-add -l fails:
  bash: `eval $(ssh-agent)`
  may also have to add the key: `ssh-add ~/.ssh/id_ed25519_github` or whatever key you want to use

There's a way to start the agent on login but I can't be bothered to look it up at the moment.

## 1password is also available 

- `ssh.exe`
- `ssh-add.exe -l`

still haven't found a good way to tell it what key to use when though.
