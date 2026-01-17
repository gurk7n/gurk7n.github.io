---
title: "How to Upgrade Reverse Shell to Fully Interactive TTY with Python"
date: 2025-09-02 10:58:00 +0300
image: /assets/img/posts/python-reverse-shell-upgrade/cover.jpg
categories: [Post-Exploitation]
tags: [reverse shell, post-exploitation, python, ctf tips]
---

When you obtain a reverse shell, you typically encounter a limited environment. For example, you cannot use `Ctrl+C` to cancel commands,  `Tab` completion for paths doesn't work, and the `clear` ommand fails to clean the terminal screen. Fortunately, Python’s `pty` module can be used to spawn a more functional, interactive shell.

> This method requires Python to be installed on the target machine.
{: .prompt-warning }

## Spawning the PTY

First, use the PTY module to spawn a `/bin/bash` shell:

```bash 
python -c 'import pty; pty.spawn("/bin/bash")'
```

![Spawning an interactive shell using Python PTY module](/assets/img/posts/python-reverse-shell-upgrade/img1.png){: width="972" height="589" }

This step makes the shell more interactive, but some limitations like `Ctrl+C` behavior—still persist. To fix this, we need to adjust our local terminal settings.

## Configuring Raw Mode

To enable raw mode, first background the shell by pressing `Ctrl+Z`. Then, update your local terminal settings and bring the shell back to the foreground:

```bash
stty raw -echo; fg
```

> In some connection types, `Ctrl+Z` might terminate the session. If this happens, skip this step.
{: .prompt-tip }

## Setting the TERM Variable

Finally, to ensure tools like `nano` or `top` work correctly, define the `TERM` environment variable:

```bash
export TERM=xterm
```

Congratulations! You now have a fully functional terminal experience, allowing for much more efficient post-exploitation and system analysis.