# GIT

## Git prompt

The git prompt utility provides a command line prompt with current git branch and status

1. Download https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh
2. `vim ~/.bash_profile`
3. Paste the following into bash profile  
`source ~/git-prompt.sh`  
  `export GIT_PS1_SHOWUPSTREAM="verbose name"`  
  `export GIT_PS1_SHOWCOLORHINTS=true`  
  `export GIT_PS1_SHOWDIRTYSTATE=true`  
  `export GIT_PS1_SHOWSTASHSTATE=true`  
  `export GIT_PS1_DESCRIBE_STYLE=describe`  
  `export GIT_PS1_SHOWUNTRACKEDFILES=true`    
    
`export PS1='\[\033[38m\]\u@\H\[\033[00m\]:\[\033[32m\]\w\[\033[33m\]$(__git_ps1)\[\033[31m\] [\j] [\!]\[\033[00m\]$ '`