# Codex Sandbox Setup on Ubuntu

Codex uses bubblewrap (`bwrap`) for its Linux sandbox and needs unprivileged user namespaces. On Ubuntu 24.04+, AppArmor restricts user namespace creation for unconfined programs, so bubblewrap can be installed and still fail. Ubuntu's mitigation model is to allow specific applications through AppArmor profiles with a `userns,` rule rather than disabling restrictions globally.

Sources:
- https://ubuntu.com/blog/ubuntu-23-10-restricted-unprivileged-user-namespaces
- https://ubuntu.com/server/docs/how-to/security/apparmor/

## Install Node via nvm

Codex requires Node.js. Install via nvm to avoid distro package staleness:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
nvm install --lts
```

## Install Codex

```bash
npm install -g @openai/codex
```

## Install bubblewrap

```bash
sudo apt-get update && sudo apt-get install -y bubblewrap
```

If AppArmor tooling isn't present but AppArmor is loaded:

```bash
sudo apt-get install -y apparmor
```

## Configure user namespace sysctls

Write `/etc/sysctl.d/90-codex-sandbox.conf`:

```ini
# Enable unprivileged user namespace creation (if the key exists on your kernel)
kernel.unprivileged_userns_clone = 1

# Ensure enough namespaces are available
user.max_user_namespaces = 28633
```

Apply:

```bash
sudo sysctl -p /etc/sysctl.d/90-codex-sandbox.conf
```

Not all kernels expose both keys. Check with `sysctl -n <key>` before writing — skip any key that doesn't exist.

## AppArmor profile for bwrap

Only needed when AppArmor restricts unprivileged user namespaces:

```bash
cat /proc/sys/kernel/apparmor_restrict_unprivileged_userns
# If this returns "1", you need the profile
```

Write `/etc/apparmor.d/codex-bwrap`:

```
abi <abi/4.0>,

include <tunables/global>

/usr/bin/bwrap flags=(default_allow) {
  userns,

  include if exists <local/codex-bwrap>
}
```

Load it:

```bash
sudo apparmor_parser -r /etc/apparmor.d/codex-bwrap
```

If another profile for `/usr/bin/bwrap` already exists in `/etc/apparmor.d/`, add `userns,` to that profile instead of creating a new one.

## Verify

```bash
bwrap --unshare-user --unshare-net --ro-bind / / /usr/bin/true && echo "OK"
```

If this fails, check:

```bash
sysctl kernel.unprivileged_userns_clone
sysctl user.max_user_namespaces
sysctl kernel.apparmor_restrict_unprivileged_userns
sudo journalctl -k -g DENIED --no-pager
```

## Codex config

Codex needs writable roots configured in `~/.codex/config.toml`:

```toml
[sandbox_workspace_write]
writable_roots = [
  "/path/to/your/project",
]
```

Add whatever directories Codex should be able to write to.
