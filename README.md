# p10k-segments

Add to your ~/.p10k.zsh inside of the `() {` section. Refer to `prompt_example` segment example in ~/.p10k.zsh.

### tsh_profile segment - @plinde

```
### tsh_profile segment - @plinde

  function prompt_tsh_profile() {
    # p10k segment -b 1 -f 3 -i '⭐' -t 'hello, %n'
    local tsh_profile=$(cat /tmp/tsh-current-profile)
    p10k segment -b 8 -t "🐚: ${tsh_profile}"
  }

    typeset -g POWERLEVEL9K_TSH_PROFILE_SHOW_ON_COMMAND='tsh|tctl|tsu'
```

(Optional) - Define your last Teleport profile login in a file so you don't need to run `tsh` repeatly. Ths is preferreable to an environment variable as it is isn't limited to your current shell. Use a shell alias to wrap your `tsh login` + `tsh-get-current-profile-segment`.

```
function tsh-get-current-profile-segment() {
    tsh status | grep "> Profile" | awk '{print $4}' | awk -F '.' '{print $2}'
}

alias tsh-login='tsh login --proxy=teleport.example.com && tsh-get-current-profile-segment > /tmp/tsh-current-profile'
```

Alternative which displays the Cluster name rather than attempt to parse something meaningful from the URL. More reliable but possibly less future-proof if Teleport decides changes the command output.

```
function tsh-get-current-profile-cluster() {
    tsh status | grep '> Profile' -A 2 | grep Cluster | awk '{print $2}'
    # e.g. Production, Staging, Dev

alias tsh-login='tsh login --proxy=teleport.example.com && tsh-get-current-profile-cluster > /tmp/tsh-current-profile'
```

### vault segment - @plinde

```
### vault segment - @plinde

    function prompt_vault() {
    # p10k segment -b 1 -f 3 -i '⭐' -t 'hello, %n'
    # local tsh_profile=$(cat /tmp/tsh-current-profile-segment)
    local vault=$(echo $VAULT_ADDR)
    p10k segment -b 8 -t "🔐: ${vault}"
  }

    typeset -g POWERLEVEL9K_VAULT_SHOW_ON_COMMAND='vault'
```

Add them to your left or right prompt elements

```
  typeset -g POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(
    # =========================[ Line #1 ]=========================
    # os_icon               # os identifier
    dir                     # current directory
    vcs                     # git status
    kubecontext
    tsh_profile
    vault
    )
```

```
  typeset -g POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(
    status                  # exit code of the last command
    command_execution_time  # duration of the last command
    background_jobs         # presence of background jobs
    tsh_profile
    vault
    )
```

