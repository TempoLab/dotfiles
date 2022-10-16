# Dotfiles 💻

This repo is largely modelled off [this guide](https://germano.dev/dotfiles/).

Criteria of this repo:

- Single repo
- Backed by `git`
- Introduce as few tools/abstractions as possible (VCSH)
- Clear way to view the commit history for any given _category_ of tools
- Ability to **optionally** bootstrap system with Ansible but also use individual configs without requiring Ansible

## Show me the code 🥵

View [all branches](https://github.com/chopfitzroy/dotfiles-experiment/branches) for each config.

## Using Ansible 🍃

If you are on WSL and using openSUSE run:

```sh
ansible-playbook main.yml --ask-become-pass
```

## Usage 🔮

Common scenarios and how to handle them are listed below. You will need to substitute in your own repository location where appropiate.

Rember even though each config is a branch on the remote. **Locally each branch will be initialized as it's own repo**.

### New host 👽

Install both [vcsh](https://github.com/RichiH/vcsh) and [myrepos](https://myrepos.branchable.com/).

**Mac:**

```sh
brew install vcsh myrepos
```

**Ubuntu:**

```sh
sudo apt-get install vcsh myrepos
```

**Clone down the `myrepos` branch:**

```sh
vcsh clone -b myrepos git@github.com:chopfitzroy/dotfiles-experiment.git myrepos
```

**Link desired tools:**

```sh
mkdir ~/.config/mr/config.d/
cd ~/.config/mr/config.d/
ls ../available.d/ # This will print out available configs
ln -s ../available.d/zsh.vcsh # This would setup ZSH config, rinse and repeat for all desired tools
```

**Checkout all the other tools at once:**

```sh
mr checkout
```

### Updating an existing tool/config 💉

```sh
vcsh enter config_name
git status # If you want to confirm changes
git add --all .
git commit -m "Commit message"
git push
```

### Setting up a new tool/config ✨

**Create new _local_ repo:**

```sh
vcsh init config_name
vcsh enter config_name
git remote add origin git@github.com:chopfitzroy/dotfiles-experiment.git
git fetch
git checkout --orphan config_name
```

**Add ignored files:**

```sh
hx ~/.gitignore.d/config_name
```

- Always start with `*`
- Add `!` paths for files you want to track

**Commit new files:**

```sh
vcsh enter config_name
git add --all .
git status # If you want to confirm your ignore paths are working as expected
git commit -m "Commit message"
git push --set-upstream origin config_name
```

**Add `myrepos` config:**

```sh
hx ~/.config/mr/available.d/config_name.vcsh
```

Then paste the following in **pay attention to replacing config_name**:

```
[$HOME/.config/vcsh/repo.d/config_name.git]
checkout =
  vcsh clone -b config_name git@github.com:chopfitzroy/dotfiles-experiment.git config_name
status = vcsh config_name status
pull = vcsh config_name pull
```

Once that is done it can be pushed to the remote:

```sh
vcsh enter myrepos
git add --all .
git commit -m "Commit message"
git push
```

## Shell 🐚

Below are a list of what I consider _must haves_ for a pleasent shell experience.

- [fd](https://github.com/sharkdp/fd)
- [fzf](https://github.com/junegunn/fzf)
- [Zoxide](https://github.com/ajeetdsouza/zoxide)
- [Sheldon](https://github.com/rossmacarthur/sheldon)
- [Starship](https://starship.rs/)

Additionally here is a list of _nice to haves_ if you don't mind adding a few more tools.

- [exa](https://the.exa.website/)
- [bat](https://github.com/sharkdp/bat)

## Reasoning 🤔

Below you can find my (personal) reasoning for each config.

This is highly subjective and there is no _perfect config_, these are just what work for me.

You might notice that for a lot of tools the config is relatively minimal **this is intentional** I am a big fan of zero config or low config tools.

I am much more likely to pick a tool that covers 90% of my use cases out of the box with sane defaults over a tool that covers 100% of my use cases but takes weeks to setup and configure.

### Helix 🧬

[Helix](https://helix-editor.com/) is a [Kakoune](https://kakoune.org/) inspired text editor. It is a modal editor similar to Vim but has some key differences.

Essentially Helix allows me to be productive fast and completely removes the need to manage plugins.

### Zsh 🐚

My primary reason for using Zsh over something like [Fish](https://fishshell.com/) is that Zsh is Bash compatiable, meaning it _just works_ with a lot of tools I already use.

If I was going to learn a new shell and commit to something that was not Bash compatiable, I would most likely pick up [Nushell](https://www.nushell.sh/) which also provides an _escape hatch_ to fallback to Bash commands when needed.
 
I use the [Sheldon](https://sheldon.cli.rs/) plugin manager because it keeps my plugin config _outside_ of my `.zshrc`, this creates a clear separation between what is my config and what is plugins.

I use [Starship prompt](https://starship.rs/) because it keeps my prompt config outside of my `.zshrc`, additionally it supports a number of different shells meaning if I ever switch shells I can likely bring this with me.

### Terminals 🧶

I use [Kitty](https://sw.kovidgoyal.net/kitty/) on Mac and Linux and [Windows Terminal](https://github.com/microsoft/terminal) on Windows (with [WSL](https://learn.microsoft.com/en-us/windows/wsl/about)).

I **do not** use [tmux](https://github.com/tmux/tmux/wiki) nor do I intend to. Terminal multiplexers come with a [plethora of issues](https://github.com/kovidgoyal/kitty/issues/391#issuecomment-638320745) that I don't need to buy into. Don't get me wrong there are absolutely uses cases for requiring a terminal multiplexer, but tabs and splits are not it. It's 2022 please let your terminal be your terminal.

## Themeing 🎨

Themeing almost entirely comes down to personal preference and the below section is very personalised to my own taste, I encourage anyone trying out this repo to experiment with the below options to find something that works for them.

### Fonts 🆎

Starship does require the use of a [nerd font](https://www.nerdfonts.com/) it is worth browsing the options to find what works best for you but some that I really enjoy are:

- [JetBrains Mono](https://www.programmingfonts.org/#jetbrainsmono)
- [Cascadia Code](https://www.programmingfonts.org/#cascadia-code)
- [Hasklig](https://www.programmingfonts.org/#hasklig)

### Color scheme 🌈

Helix includes a number of great themes out of the box, use `:theme` to find the theme that best suits you.

You may notice that I include a custom `gruvbox_dark` theme with the Helix config, this is to address [this issue](https://github.com/morhetz/gruvbox/issues/15) if you do not mind inverted colors feel free to remove this in favour of the Helix default `gruvbox`.

If you are like me and want your terminal color scheme to match your editor color scheme I can recommend the [official kitten](https://sw.kovidgoyal.net/kitty/kittens/themes/) for Kitty.

Provided you know the theme you are after you can run the following command to overwrite the current theme:

```sh
kitty +kitten themes --dump-theme Gruvbox\ Dark > ~/.config/kitty/current-theme.conf
```

Where `Gruvbox\ Dark` is the theme name. Use `ctrl` + `shift` + `F5` to reload the terminal.

If you are using Windows Terminal you will need to follow the below steps to add the Gruvbox Dark theme:

- Open Windows Terminal
- Open "Settings" (Ctrl+,)
- Click Open JSON file (bottom left)
- Search for `"schemes"`
- Copy and paste the below snippet into the array
- Navigate to the relevant profile in Windows Terminal
- Under "Additional Settings" select "Appearance"
- Under "Color Scheme" select "Gruvbox Dark"

<details>
  <summary>Click to expand snippet</summary>
  
  ```json
  {
    "background" : "#282828",
    "black" : "#282828",
    "blue" : "#458588",
    "brightBlack" : "#928374",
    "brightBlue" : "#83A598",
    "brightCyan" : "#8EC07C",
    "brightGreen" : "#B8BB26",
    "brightPurple" : "#D3869B",
    "brightRed" : "#FB4934",
    "brightWhite" : "#EBDBB2",
    "brightYellow" : "#FABD2F",
    "cyan" : "#689D6A",
    "foreground" : "#EBDBB2",
    "green" : "#98971A",
    "name" : "Gruvbox Dark",
    "purple" : "#B16286",
    "red" : "#CC241D",
    "white" : "#A89984",
    "yellow" : "#D79921"
  }
  ```
</details>

If the default themes are not enough for you I highly recommend checking out the [Base16 Project](https://github.com/base16-project) which includes a number of custom themes for both Helix and fzf.

## Gotcha's 💢

If you are running Ubuntu on WSL the `apt` repositories are often a few version behind for some tools, as such I recommend downloading either the `.deb` installers or compiled binaries directly from GitHub.

- [fd](https://github.com/sharkdp/fd)
- [exa](https://the.exa.website/)
- [bat](https://github.com/sharkdp/bat)
- [fzf](https://github.com/junegunn/fzf/releases)
- [Helix](https://github.com/helix-editor/helix/releases)

If you don't know where to put these tools I recommend `~/.local/bin` which already exists for `zoxide`.

