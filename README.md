# dotfiles

My dotfiles, tracked in a bare git repository. No magic, no ceremony.

Technique borrowed from [Nicola Paolucci's blog post](https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/).

The instructions below assume you are using `zsh`, although this is easily
adaptable for Bash.

## Starting from scratch

To bring your existing home directory under version control, initialize a bare
git repository there and define an alias to help with managing it:

```sh
git init --bare $HOME/.dotfiles
alias dotfiles='/usr/local/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
dotfiles config --local status.showUntrackedFiles no
```

Be sure to add the alias to `.zshrc`, either manually or like this:

```
echo "alias alias dotfiles='/usr/local/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'" >> $HOME/.zshrc
```

Now, you can run ordinary git commands in your home directory using your newly
created `dotfiles` alias:

```sh
dotfiles status
dotfiles add .zshrc
dotfiles commit -m "Add zshrc"
dotfiles push
```

## Installing this repository on an existing system

First, establish the dotfiles alias in `.zshrc`:

```sh
alias dotfiles='/usr/local/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
```

Ensure that your `.dotfiles` directory will be ignored by Git (to eliminate
recursion issues) by adding to `.gitignore`:

```sh
cd ~
touch .gitignore
echo ".dotfiles" >> .gitignore
```

Then, clone the repository into your home directory:

```sh
git clone --bare https://github.com/djreimer/dotfiles.git $HOME/.dotfiles
```

Checkout the content of the repository into `$HOME`:

```sh
dotfiles checkout
```

If there are conflicts with existing files, Git will let you know like this:

```
error: The following untracked working tree files would be overwritten by checkout:
    .bashrc
    .gitignore
Please move or remove them before you can switch branches.
Aborting
```

Be sure to back those up first before moving forward.

## Bootstrapping

Once directory is under git management, run the bootstrap script to
perform additional environment setup (tap Homebrew bundle, install Zsh, etc.):

```
script/bootstrap
```
