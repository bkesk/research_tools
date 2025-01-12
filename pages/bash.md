# Bash/command line

You should know about .bashrc, .bash_profile. The .bash_profile is loaded only on non-interactive sessions. You can customize everything, people have examples online.

`!py` will run last command starting with ‘py’.

`<ctrl -r>` reverse search of previous commands.

How to change history size, e.g.
```
# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000
```

Exporting bash functions is better than alias, as it is accessible outside of the bash command line. 

`.ssh/config` lets you avoid entering in addresses, i.e. `ssh rusty` will work. See `Host` keyword.
```
 Host flatiron
   Hostname gateway.flatironinstitute.org
   Port 61022
   User your_username
   ForwardX11 yes 
   ForwardX11Trusted yes 
   DynamicForward 127.0.0.1:61080   # This enables port forwarding through the SSL tunnel (more below)
   ControlPath ~/.ssh/.%r@%h:%p     # (with 'ControlMaster auto' set) specifies the localation of the control socket for ssh multiplexing (See below)
   ControlMaster auto               # This allows multiple SSH sessions to use same SSL tunnel so that you don't have to log in again (called ssh multiplexing)
   ServerAliveInterval 100          # stop the connection from closing automatically.
```

Port forwading ('DynamicForward' option above) is very general and can be used for many different things, including a similar effect to ProxyJump (see below). Port forwarding can be used to forward traffice to a different host:port combination. More on ssh port forwarding: https://www.redhat.com/sysadmin/ssh-dynamic-port-forwarding

SSH Multiplexing ('ControlMaster','ControlPath',etc.) is focused scpeficially on allowing multiple ssh sessions to share the same "master" session. As long as the master session is active, new ssh sessions can simply connect automatically without having to log in again. This is very useful when combined with Proxy-jumping (below). It is a good security practice to close the master session when you won't be connecting again for a while (like at the end of the day). More on on ssh multiplexing: https://www.techrepublic.com/article/how-to-use-multiplexing-to-speed-up-the-ssh/

Proxy-jump to iterate login commands (a la Bryan)
```
Host flatiron
  Hostname gateway.flatironinstitute.org
  Port 61022
Host rusty rustyvflatiron
  Hostname rusty.flatironinstitute.org
Host rustyvflatiron
  ProxyJump flatiron
```


ssh-keygen will allow you to generate keys, for passwordless login (if the admin allows it).

`rsync` is faster than `scp` in most cases, and has a bit more control (exclude file patterns, …). Sometimes I use it instead of `cp` for local!
`-a` option is super useful, it keeps modification times, among other things.

[Custom command line scripts](bin_scripts.md) that you can call like other bash commands. 

`PATH`, `PYTHONPATH`, `LD_MODULE_PATH` are useful environment variables. `module` (see [module notes](module.md)) is probably the best tool to edit these. Be very careful messing with these, it can cause compilation problems, if the wrong libraries are being used. Robert in SCC is setting up `spack`, a newer way of handling it. See Slack.

Piping (`|`):
```
ls -1 scripts_sweep/*.slurm.sh | xargs -L 1 sbatch
```

Using ticks (OR `$(...)` ) to save the results.
```
all_py_files=`ls */*py`          # OR  all_py_files=$(ls */*py)
echo $all_py_files
```

Bash alias/functions. E.g. show git branch on prompt (PS1)
```
parse_git_branch () {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1) / '
}
PS1='[\u@\h \W]$(parse_git_branch)\$ '
```

Iteration over values in bash.
```
ls {this, that, the_other}.out   # will try to ls three files in that order. 
ls runs{0..9}.out                # will try to ls 10 files from 0 to 9 (I think inclusive)
```

# ZSH

Bash superset. ‘Oh my ZSH’ on github, package manager. Grep automatically at the shell.

