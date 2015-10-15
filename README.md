# Vagrant / Rails dev box

This is a sample config for a dev system based on a virtual machine running Ubuntu 14.04 with rbenv and bundler installed.

## Install

Note: *If you're starting from a clean machine a good way to start is to install all the required stuff via my dotfiles: [https://github.com/victormier/dotfiles](https://github.com/victormier/dotfiles) with (this installs some other stuff):*

```
git clone https://github.com/victormier/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
script/bootstrap
brew/install.sh
brew-cask/install.sh
```

Things you'll need to install:

- vagrant
- virtualbox

Install via homebrew. We'll install homebrew, brew-cask, vagrant and virtualbox:

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install caskroom/cask/brew-cask
brew cask install vagrant
brew cask install virtualbox
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-librarian-chef-nochef
```

Now clone the repo and bring the vagrant machine up:

```
git clone https://github.com/victormier/vagrant-rails-test.git
cd vagrant-rails-test
vagrant up
vagrant provision
```

Now you can access to the machine with:

```
vagrant ssh
```

There's a shared folder in the guest machine (`/vagrant`) which contains the synced content of your host machine project folder:

```
cd /vagrant
```
