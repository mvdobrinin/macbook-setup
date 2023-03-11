# mvdobrinin's Mac OS X 13.2 macOS Ventura Setup Guide

Blatantly stealing the idea from Kevin Elliott's [El Capitan Guide](https://gist.github.com/kevinelliott/e12aa642a8388baf2499), I've decided to document as much as I can of my new computer setup guide. There's a lot to do when refreshing a computer or setting one up from scratch, but a bit of planning reduces a ton of pain later on. :relaxed:

If there are steps that you've noticed that I'm clearly missing, please let me know. If you want to fork this guide to make your own, go right ahead!

## Before Reformat

I generally am doing this because I'm reformatting an old computer because I have a problem (usually the computer, always me). I sometimes forget that there are more than files to backup, since not everything syncs perfectly. Here's what I need to remember to sync and where they live.

- Chrome - OneTab should be bookmarked, and the rest Chrome syncs itself
- iTerm2 - Syncing preferences to Dropbox
- VS Code - Syncs with VsCode sync built in feature


## Install Basic Software

This is the software that I use on a very regular basis. Not all software is listed, as this would be one of the most time consuming to keep up to date.

### Install from App Store/Web
- [VS Code](https://code.visualstudio.com/download)
- [Monitor Control](https://github.com/MonitorControl/MonitorControl)
- [Better Display](https://github.com/waydabber/BetterDisplay)

### Homebrew

##### Install Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

##### Install Homebrew extension Cask

```
brew tap homebrew/cask-fonts
```

##### Install common applications via Homebrew
_Yes, you can run this all as one `brew install` command followed by the list of applications, but some require additional input or could have other issues installing, so I run them separately to give an easy way to continue if needed_

```
brew install git
brew install grep
brew install highlight
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

#### Oh My ZSH!
- Install a fancy zsh framework for funtimes
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
- Make zsh the default shell for the current user with `chsh -s $(which zsh)`
- Setup changes to the .zshrc file to config for our usage

#### Make some aliases
My current zsh profile with my aliases is backed up with Mackup now, but just in case these are needed

### Set Up Applications

- Login to Chrome & Firefox to download and setup extensions
- Login to Dropbox and get files
- Make dev files that I use open in VS Code (things like .txt, .sh, .json)
- Setup Rectangle shortcuts so they don't interfere with Firefox shortcuts
- Load existing iTerm2 .plist file from Dropbox, most likely running `killall cfprefsd` with it closed to clear the cached file
- I save custom scripts in Dropbox because I would probably lose them somehow otherwise. The files here have to be sourced, and the folder has to be added to the PATH environment variable. Both of those are accomplished with this portion of my `.zshrc` file:
```
# Add my scripts folder to the path
PATH=$PATH:~/Dropbox/scripts
# Source stuff!
source ~/.zshrc

for f in ~/Dropbox/scripts/zsh/*; do
	if [[ $file == *.sh ]]
	then
		source "$f"
	fi
done
```


### Gitting on with Git
- Xcode and git are installed, right?
- If so, running `xcode-select --install` will get you the prompts for the Xcode Command Line Tools
- Set some defaults up.
```
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global github.user githubusername
git config --global push.default current
git config --global init.defaultBranch "main"
git config --global merge.conflictstyle diff3
git config --global core.editor "code -wait"
git config --global core.pager "diff-so-fancy | less --tabs=4 -RFX"
git config --global color.ui true
git config --global color.diff-highlight.oldNormal    "red bold"
git config --global color.diff-highlight.oldHighlight "red bold 52"
git config --global color.diff-highlight.newNormal    "green bold"
git config --global color.diff-highlight.newHighlight "green bold 22"
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

## System Settings

### General Settings

- Turn off all stupid notifications and badges/banners/butchers of concentration
- Set Firefox Developer Edition as the default browser
- Set Recent items to none
- Make dock nice and tiny
- Set time format to 24-hour time
- Change display energy saver settings
- Set key repeat to fast and delay until repeat to short
- Turn off keyboard brightness when computer is unused
- Setup replacement texts (like yall) so it doesn't try autocorrecting my informalities
- Set trackpad click to light, tracking speed to rather fast, and silent clicking
- Turn off launchpad trackpad gesture
- Setup internet accounts
- Show bluetooth in control center
- Show battery percentage in control center
- Show date and time in menu bar
- Ensure that guest account is off, and main account profile is set
- Show all files including hidden ones `defaults write com.apple.finder AppleShowAllFiles YES;`
- Make notification banners only display for three seconds, because ten is ridiculous. `defaults write com.apple.notificationcenterui bannerTime 3`
- Change screenshots to jpg `defaults write com.apple.screencapture type jpg`


At this point, you're probably done with computers, the internet, everything. At the very least, when you regain consciousness, your computer will be mainly good to go!
