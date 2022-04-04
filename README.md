# Guide
Personal practices, guides and how to do stuff to ease development


## How to show current git branch with colors in Bash prompt

(Taken from [here](https://thucnc.medium.com/how-to-show-current-git-branch-with-colors-in-bash-prompt-380d05a24745))

Adding current git branch name in Bash prompt with highlighted color can help
to avoid a lot of problems with working on wrong git branches, which is a quite
popular scenario to most of the developers sometime.

Basically add these lines to the end of your `~/.bashrc`:

```bash
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
export PS1="\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\e[91m\]\$(parse_git_branch)\[\e[00m\]$ "
```
In the given link the following string is used for `PS1` but, I don't like how
it changes the colors and adds a lot of spaces on the terminal line (so I use the one
given above).

```bash
export PS1="\u@\h \[\e[32m\]\w \[\e[91m\]\$(parse_git_branch)\[\e[00m\]$ "
```

Under ubuntu you can replace the lines for setting the PS1 and addint the git parsing
func

```bash
# This is the old PS1
# if [ "$color_prompt" = yes ]; then
#     PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
# else
#     PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
# fi

parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\e[91m\]\$(parse_git_branch)\[\e[00m\]$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$(parse_git_branch)$ '
fi
unset color_prompt force_color_prompt
```

After editing the `.bashrc` you can restart the terminal or run the following line
to imediately load the new configuration:

```console
$ source ~/.bashrc
```

### How to restore the default `.bashrc`

If you went to experiment with changing `.bashrc` on your own it is good to know
how can you restore the default one. There exist backup copies of `.bashrc`, 
`.profile` etc. in `/etc/skel/`. One could simply do it by overwitting from there.

## Useful git aliases

When using git on regular basis one usualy has the need to have a more clearer view
of the git tree. The `git log` command has a lot of details and doesn't show much
of the connection between branches and commits in history. This is why I use the 
following settings for my git. 

On Linux machines the git configuration file is at the home folder `~/.gitconfig`.
This is the settings that I use:

``` ini
[alias]
    tracked = ls-tree -r master --name-only
    ignored = ls-files . --ignored --exclude-standard --others
    tree = log --graph --abbrev-commit --decorate --date=relative --all -20 --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)'
    bigtree = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset%n' --abbrev-commit --date=relative --branches	
[user]
    name = Ginko Balboa
    email = ginkobalboa3@gmail.com
[core]
    editor = gedit -s
```

