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

After editing the `.bashrc` you can restart the terminal or run the following line
to imediately load the new configuration:

```console
$ source ~/.bashrc
```

### How to restore the default `.bashrc`

If you went to experiment with changing `.bashrc` on your own it is good to know
how can you restore the default one. There exist backup copies of `.bashrc`, 
`.profile` etc. in `/etc/skel/`. One could simply do it by overwitting from there.
