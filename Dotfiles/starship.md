---
tags:
  - dotfile
create_time: 2024-10-25 15:49
modified_time: 2024-10-25 15:49
status: complete
---

## main
```toml
format = """
$os\
$username\
$hostname\
$directory\
$git_branch\
$git_state\
$git_status\
$fill\
$c\
$rust\
$golang\
$nodejs\
$php\
$java\
$kotlin\
$haskell\
$python\
$docker_context\
$time\
$line_break$character"""

add_newline = false

[fill]
symbol = " "

[os]
disabled = false

[os.symbols]
Windows = " "
Ubuntu = "󰕈 "
SUSE = " "
Raspbian = "󰐿 "
Mint = "󰣭 "
Macos = " "
Manjaro = "󱘊 "
Linux = "󰌽 "
Gentoo = "󰣨 "
Fedora = "󰣛 "
Alpine = " "
Amazon = " "
Android = " "
Arch = "󰣇 "
Artix = "󰣇 "
CentOS = " "
Debian = "󰣚 "
Redhat = "󱄛 "
RedHatEnterprise = "󱄛 "

[username]
style_user = "green bold"
style_root = "red bold"
format = "[$user]($style) "
disabled = false
show_always = true

[hostname]
ssh_only = false
format = 'on [$hostname](bold purple) '
trim_at = "."
disabled = false

# Replace the "❯" symbol in the prompt with "➜"
[character]
success_symbol = "[➜](bold green)"
error_symbol = "[✗](bold red)"


# configure directory
[directory]
read_only = "  "
truncation_length = 10
truncation_symbol = "…/"
truncate_to_repo = true # truncates directory to root folder if in github repo
style = "bold italic blue"

[cmd_duration]
min_time = 4
show_milliseconds = false
disabled = false
style = "bold italic red"

[aws]
symbol = " "

[conda]
symbol = " "

[dart]
symbol = " "

#[directory]
#read_only = " "

[docker_context]
symbol = " "
format = "via [$symbol$context]($style) "
style = "blue bold"
only_with_files = true
detect_files = ["docker-compose.yml", "docker-compose.yaml", "Dockerfile"]
detect_folders = []
disabled = false

[elixir]
symbol = " "

[elm]
symbol = " "

[git_branch]
symbol = " "

[git_status]
format = '([\[$all_status$ahead_behind\]]($style) )'
stashed = "[${count}*](green)" 
modified = "[${count}+](yellow)" 
deleted =  "[${count}-](red)" 
conflicted =  "[${count}~](red)" 
ahead = "⇡${count}"
behind = "⇣${count}"
untracked = "[${count}?](blue)" 
staged = "[${count}+](green)" 

[git_state]
style =	"bold red"
format = "[$state( $progress_current/$progress_total) ]($style)"
rebase = "rebase"
merge = "merge"	
revert = "revert"	
cherry_pick = "cherry"
bisect = "bisect"	
am = "am"	
am_or_rebase = "am/rebase"

[c]
symbol = " "

[rust]
symbol = " "

[golang]
symbol = " "

[nodejs]
symbol = " "
#format = "via [ Node.js $version](bold green) "
#detect_files = ["package.json", ".node-version"]
#detect_folders = ["node_modules"]

[hg_branch]
symbol = " "

[php]
symbol = " "

[java]
symbol = " "

[kotlin]
symbol = " "

[julia]
symbol = " "

[haskell]
symbol = "λ "

[memory_usage]
symbol = " "

[nim]
symbol = " "

[nix_shell]
symbol = " "

[package]
symbol = " "

[perl]
symbol = " "


[python]
symbol = "🐍 "
#pyenv_version_name = true
format = 'via [${symbol}python (${version} )(\($virtualenv\) )]($style)'
pyenv_prefix = "venv "
python_binary = ["./venv/bin/python", "python", "python3", "python2"]
detect_extensions = ["py"]
version_format = "v${raw}"

[ruby]
symbol = " "

[scala]
symbol = " "

[shlvl]
symbol = " "

[swift]
symbol = "ﯣ "

[time]
disabled = false
time_format = "%R"
style = "#b5b6b1"
format = '[ $time ]($style)'
```


## 仿p10k
```toml
"$schema" = 'https://starship.rs/config-schema.json'

format = """
[](color_orange)\
$os\
$username\
[](bg:color_yellow fg:color_orange)\
$directory\
[](fg:color_yellow bg:color_aqua)\
$git_branch\
$git_status\
[](fg:color_aqua bg:color_blue)\
$c\
$rust\
$golang\
$nodejs\
$php\
$java\
$kotlin\
$haskell\
$python\
[](fg:color_blue bg:color_bg3)\
$docker_context\
[](fg:color_bg3 bg:color_bg1)\
$time\
[ ](fg:color_bg1)\
$line_break$character"""

palette = 'gruvbox_dark'

[palettes.gruvbox_dark]
color_fg0 = '#fbf1c7'
color_bg1 = '#3c3836'
color_bg3 = '#665c54'
color_blue = '#458588'
color_aqua = '#689d6a'
color_green = '#98971a'
color_orange = '#d65d0e'
color_purple = '#b16286'
color_red = '#cc241d'
color_yellow = '#d79921'

[os]
disabled = false
style = "bg:color_orange fg:color_fg0"

[os.symbols]
#Windows = "󰍲"
Windows = "󰌽"
Ubuntu = "󰕈"
SUSE = ""
Raspbian = "󰐿"
Mint = "󰣭"
Macos = "󰀵"
Manjaro = ""
Linux = "󰌽"
Gentoo = "󰣨"    
Fedora = "󰣛"
Alpine = ""
Amazon = ""
Android = ""
Arch = "󰣇"
Artix = "󰣇"
CentOS = ""
Debian = "󰣚"
Redhat = "󱄛"
RedHatEnterprise = "󱄛"
Pop = ""

[username]
show_always = true
style_user = "bg:color_orange fg:color_fg0"
style_root = "bg:color_orange fg:color_fg0"
format = '[ $user ]($style)'

[directory]
style = "fg:color_fg0 bg:color_yellow"
format = "[ $path ]($style)"
truncation_length = 3
truncation_symbol = "…/"

[directory.substitutions]
"Documents" = "󰈙 "
"Downloads" = " "
"Music" = "󰝚 "
"Pictures" = " "
"Developer" = "󰲋 "

[git_branch]
symbol = ""
style = "bg:color_aqua"
format = '[[ $symbol $branch ](fg:color_fg0 bg:color_aqua)]($style)'

[git_status]
style = "bg:color_aqua"
format = '[[($all_status$ahead_behind )](fg:color_fg0 bg:color_aqua)]($style)'

[nodejs]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[c]
symbol = " "
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[rust]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[golang]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[php]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[java]
symbol = " "
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[kotlin]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[haskell]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[python]
symbol = ""
style = "bg:color_blue"
format = '[[ $symbol( $version) ](fg:color_fg0 bg:color_blue)]($style)'

[docker_context]
symbol = ""
style = "bg:color_bg3"
format = '[[ $symbol( $context) ](fg:#83a598 bg:color_bg3)]($style)'

[time]
disabled = false
time_format = "%R"
style = "bg:color_bg1"
format = '[[  $time ](fg:color_fg0 bg:color_bg1)]($style)'

[line_break]
disabled = false

[character]
disabled = false
success_symbol = '[](bold fg:color_green)'
error_symbol = '[](bold fg:color_red)'
vimcmd_symbol = '[](bold fg:color_green)'
vimcmd_replace_one_symbol = '[](bold fg:color_purple)'
vimcmd_replace_symbol = '[](bold fg:color_purple)'
vimcmd_visual_symbol = '[](bold fg:color_yellow)'

[character]
disabled = false
success_symbol = '[](bold fg:green)'
error_symbol = '[](bold fg:red)'
vimcmd_symbol = '[](bold fg:creen)'
vimcmd_replace_one_symbol = '[](bold fg:purple)'
vimcmd_replace_symbol = '[](bold fg:purple)'
vimcmd_visual_symbol = '[](bold fg:lavender)'
```

