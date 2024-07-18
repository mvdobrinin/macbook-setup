# mvdobrinin's Mac OS X 14.3 macOS Sonoma Setup Guide

## Install Basic Software
  
### Homebrew

##### Install Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
/opt/homebrew/bin/brew doctor
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
```

### Install common applications via Homebrew
_Yes, you can run this all as one `brew install` command followed by the list of applications, but some require additional input or could have other issues installing, so I run them separately to give an easy way to continue if needed_

```
brew tap homebrew/cask-fonts
```

```
brew install iterm2
brew install git
brew install nmap
brew install jenv
brew install pyenv
brew install wget
brew install zsh-completions
brew install zsh-syntax-highlighting
```

### Install applications via Homebrew Cask

```
brew install font-roboto font-roboto-mono font-source-code-pro
brew install --cask font-jetbrains-mono
```

### DB tools for Postgres

```
brew install libpq
echo 'export PATH="/opt/homebrew/opt/libpq/bin:$PATH"' >> ~/.zshrc
```

### Oh My ZSH!
- Install a fancy zsh framework for funtimes
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
- Make zsh the default shell for the current user with `chsh -s $(which zsh)`
- Setup changes to the .zshrc file to config for our usage

### Additional Command Line Installs

### Set up VSCode

Go to Code - Settings... - Turn on Settings Sync...
Sign in with Github

### Install JDKs

Download JDKs of choice. For example through IJ.

```
jenv add /Users/dobrim1/Library/Java/JavaVirtualMachines/corretto-17.0.10/Contents/Home
```

#### Install NodeJS and switcher tool

We use [nvm tool](https://github.com/nvm-sh/nvm?tab=readme-ov-file#install--update-script) to be able to install and support multiple NodeJS versions.

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
