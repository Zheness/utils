There is some files/config I use.

## .bashrc

My personal `.bashrc`, with my custom prompt and aliases.

```bash
# Root
# PS1='\[\e[1;31m\][\u@\h \W]\$\[\e[0m\] '
# User
PS1='[\u@\h \W]\$ '

export LS_OPTIONS='--color=auto'
eval "`dircolors`"
alias ls='ls $LS_OPTIONS'
alias l='ls -l'
alias la='ls -la'
alias md='mkdir'
alias ..='cd ..'
alias j='jobs'
```
