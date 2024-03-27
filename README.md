# mvdobrinin's Mac OS X 13.2 macOS Ventura Setup Guide

Blatantly stealing the idea from Kevin Elliott's [El Capitan Guide](https://gist.github.com/kevinelliott/e12aa642a8388baf2499), I've decided to document as much as I can of my new computer setup guide. There's a lot to do when refreshing a computer or setting one up from scratch, but a bit of planning reduces a ton of pain later on. :relaxed:

If there are steps that you've noticed that I'm clearly missing, please let me know. If you want to fork this guide to make your own, go right ahead!

## Install Basic Software

This is the software that I use on a very regular basis. Not all software is listed, as this would be one of the most time consuming to keep up to date.

### Install from App Store/Web
- [Monitor Control](https://github.com/MonitorControl/MonitorControl)
- [Better Display](https://github.com/waydabber/BetterDisplay)

  
### Oh My ZSH!
- Install a fancy zsh framework for funtimes
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
- Make zsh the default shell for the current user with `chsh -s $(which zsh)`
- Setup changes to the .zshrc file to config for our usage

### Homebrew

##### Install Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
/opt/homebrew/bin/brew doctor
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
```

##### Install Homebrew extension Cask

```
brew tap homebrew/cask-fonts
```

##### Install common applications via Homebrew
_Yes, you can run this all as one `brew install` command followed by the list of applications, but some require additional input or could have other issues installing, so I run them separately to give an easy way to continue if needed_

```
brew install git
brew install htop
brew install nmap
brew install openjdk
brew install pipenv
brew install wget
brew install zsh
brew install zsh-completions
brew install zsh-syntax-highlighting
```

##### Install applications via Homebrew Cask

Seriously, barring the insertion of malicious code or lack of checksums (two things which should honestly scare me away of many), Cask is pretty useful. I'm choosing to be willfully ignorant, since broadcasting usage opens me up anyway, and this saves a lot of time.

```
brew install font-roboto font-roboto-mono font-source-code-pro
brew install --cask font-jetbrains-mono
brew install iterm2

```

### Additional Command Line Installs

### Set up VSCode

Go to Code - Settings... - Turn on Settings Sync...
Sign in with Github

#### Install NodeJS and switcher tool

We use [n tool](https://www.npmjs.com/package/n) to be able to install and support multiple NodeJS versions.

```
brew install n
sudo n latest
```

### Set Up Applications

- Login to Chrome to download and setup extensions
- Login to Bitwarden

### Gitting on with Git

- Xcode and git are installed, right?
- If so, running `xcode-select --install` will get you the prompts for the Xcode Command Line Tools
- Set some defaults up.
```
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global push.default current
git config --global init.defaultBranch "master"
git config --global color.ui true
```

Also we an set up VSCode as an excellent merge tool.
```
git config --global --edit
```

And add the following
```
[merge]
        tool = vscode
[mergetool "vscode"]
        cmd = code --new-window --wait $MERGED
[diff]
        tool = vscode
[difftool "vscode"]
        cmd = code --new-window --wait --diff $LOCAL $REMOTE                                                    
```

- Check that keychain helper is installed with `git credential-osxkeychain` **Note:** if you installed git via HomeBrew, this is done for you. Skip to the `git config` step below.
- If not installed, set that sucker up.
`curl -s -O http://github-media-downloads.s3.amazonaws.com/osx/git-credential-osxkeychain`
- Modify permissions on the helper so it can operate
`chmod u+x git-credential-osxkeychain`
- Move the helper so Git can access it. This command will ask you for your (computer user) password. As you're typing your password, it won't show the characters, press return when done typing it. `sudo mv git-credential-osxkeychain /usr/local/git/bin`
- Tell git to use the helper `git config --global credential.helper osxkeychain`
- Check again to see if the helper is successfully installed `git credential-osxkeychain`
- Create a new SSH key for Github
```
cd ~/.ssh
ssh-keygen -t rsa -b 8192 -C "your@email.com"
```
- Confirm that ssh-agent is enabled
`eval "$(ssh-agent -s)"`
- Add SSH key to ssh-agent
`ssh-add ~/.ssh/id_rsa`
- Copy SSH key to clipboard
`pbcopy < ~/.ssh/id_rsa.pub`
- Login to Github
- [Add SSH key to Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)
- Confirm that you're good to go
`ssh -T git@github.com`
