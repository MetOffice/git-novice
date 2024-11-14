---
title: Setup
---

## Pre-workshop Survey

Please remember to fill out the pre-workshop survey prior
to the start of the workshop.
This information is vital for us to keep improving the lesson
for other learners.

## Installing Git

Since several Carpentries lessons rely on Git, please see
[this section of the workshop template][workshop-setup] for
instructions on installing Git for various operating systems.

- [Git installation on Windows][workshop-setup]
- [Git installation on MacOS][workshop-setup]
- [Git installation on Linux][workshop-setup]

Your institution may have already installed git on your work computer.
Ask your instructor if this step is necessary or simply type:

```bash
$ git --version
git version 2.47.0
```

If a version number is printed like the output above git is installed and 
ready to use.

## Git Autocomplete

Git provides a script which lets us display the version control status in your 
terminal prompt.
The following instructions have been tested on **Linux**.
If you are using MacOS or Windows please consult the
[git autocomplete instructions](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh)
at the top of the linked file.
Your instructor may point you to another online resource for your OS.
To enable this script add the following to a new `~/.bashrc.d/git.bash` 
file:

```bash
if [[ $- =~ i ]]; then
    GIT_PROMPT_PATH=~/git-prompt.sh
    if [[ -r "${GIT_PROMPT_PATH}" ]]; then    
        source "${GIT_PROMPT_PATH}" >&2
    else
        if [[ "$-" == *i* ]]; then
            echo "${GIT_PROMPT_PATH} - not found" >&2
        fi
    fi
    export GIT_PS1_SHOWDIRTYSTATE=1
    export GIT_PS1_SHOWSTASHSTATE=1
    export GIT_PS1_SHOWUPSTREAM="auto"
    export GIT_PS1_SHOWCOLORHINTS=1
    export GIT_PS1_SHOWUNTRACKEDFILES=1

    export PROMPT_COMMAND=("${PROMPT_COMMAND[@]}" '__git_ps1 "${CONDA_PROMPT_MODIFIER}[\u@\h:\w]:" "\$ " "(%s)"')
    export PS1='[\u@\h:\w]$(__git_ps1 "(%s)"):\$ '

    # trim long working directory paths
    PROMPT_DIRTRIM=3
fi
```

And make sure your `~/.bashrc` file includes:

```bash
# User specific aliases and functions

if [ -d ~/.bashrc.d ]; then
	for rc in ~/.bashrc.d/*; do
		if [ -f "$rc" ]; then
			. "$rc"
		fi
	done
fi

unset rc
```

Your instructor will let you know if the value of `GIT_PROMPT_PATH` is 
different from the path in the example above.
If you would like your own copy of the `git-prompt.sh` script you can 
download the latest version from the 
[git repository contrib directory](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh).
Ensure the `GIT_PROMPT_PATH` matches where you decide to store the `git-prompt.sh` file.

If your `~/.bashrc` file, or any file in the `~/.bashrc.d/` directory,
already defines a `PROMPT_COMMAND` or `PS1`
you may need to merge what is shown above with your current script.
Your instructor may be able to help you with this.

If adding in your username, `\u`, and hostname, `\h`, makes the terminal prompt
too long you can remove the `\u` and or `\h` from the `PROMPT_COMMAND` and
`PS1` lines.

The `PROMPT_DIRTRIM=3` line is optional.
It trims long directory paths to only show the current and two parent directories.
You can change this value or delete the line entirely based on your preference.

```bash
/Desktop/A/Really/Long/Path $ # without PROMPT_DIRTRIM
.../Really/Long/Path $          # with PROMPT_DIRTRIM
```

To see the changes to your terminal prompt run:

```bash
source ~/.bashrc
```

::: callout

## Using MacOS or older versions of Bash

If your version of Bash is less than 5.1 or you are using
MacOS you might need to change the `export PROMPT_COMMAND`
line to:

```bash
export PROMPT_COMMAND=${PROMPT_COMMAND:+"$PROMPT_COMMAND; "}'__git_ps1 "${CONDA_PROMPT_MODIFIER}[\w]:" "\$ " "(%s)"'
```

:::

## Creating a GitHub Account

You will need an account for [GitHub](https://github.com) to follow episodes 7 & 8 in this lesson.

1. Go to <https://github.com> and follow the "Sign up" link at the top-right of the window.
2. Follow the instructions to create an account.
3. Verify your email address with GitHub.
4. Configure multifactor authentication and or a passkey (see below).

There is no fixed guidance for choosing your GitHub username however you should ensure it is suitable for work.
At the Met Office a common pattern for usernames is: `mo-{first name initial}{surname}`.
So if your name is `Eleanor Ormerod` your username would be: `mo-eormerod`

### Multi-factor Authentication

In 2023, GitHub introduced a requirement for 
all accounts to have 
[multi-factor authentication (2FA)](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication) 
configured for extra security.
Several options exist for setting up 2FA, which are summarised here:

1. If you already use an authenticator app, 
   like [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=en&co=GENIE.Platform%3DiOS&oco=0) 
   or [Duo Mobile](https://duo.com/product/multi-factor-authentication-mfa/duo-mobile-app) on your smartphone for example, 
   [add GitHub to that app](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-mobile-app).
2. If you have access to a smartphone but do not already use an authenticator app, install one and 
   [add GitHub to the app](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-mobile-app).
3. If you do not have access to a smartphone or do not want to install an authenticator app, you have two options:
    1. [set up 2FA via text message](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-text-messages) 
       ([list of countries where authentication by SMS is supported](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/countries-where-sms-authentication-is-supported)), or
    2. [use a hardware security key](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-security-key) 
       like [YubiKey](https://www.yubico.com/products/yubikey-5-overview/) 
       or the [Google Titan key](https://store.google.com/us/product/titan_security_key?hl=en-US&pli=1).

The GitHub documentation provides [more details about configuring 2FA](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication).

### Passkeys

To completely avoid having authentication for work purposes on a personal device you may choose to [set up a passkey](https://docs.github.com/en/authentication/authenticating-with-a-passkey/managing-your-passkeys). Your instructor and organisation will be able to provide guidance on suitable passkey providers and password managers.

[workshop-setup]: https://carpentries.github.io/workshop-template/install_instructions/#git

