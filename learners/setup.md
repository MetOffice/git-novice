---
title: Setup
---

## Lesson Outcomes

This lesson is split into two parts.
Episodes before the break focus on Git.
Episodes in the second half after the break focus on GitHub.

By the end of the Git section you will be able to:

- describe the importance of Version Control.
- describe the similarities and differences between SVN and Git.
- configure Git.
- initialise new repositories using Git.
- develop changes on a branch.
- view differences between changes and explore your repositories history.

By the end of the GitHub section you will be able to:

- set up important GitHub settings and SSH access.
- describe the similarities and differences between Trac and GitHub.
- explain the differences between private, internal and public repositories.
- set up a repository, including how to protect the `main` branch and enable
  wiki pages, discussions, and GitHub projects.
- view a repositories history and differences between changes.
- view Issues and Pull Requests.
- contribute changes to a repository using a Pull Request.

And much more!

## Pre-lesson Survey

Please remember to fill out the
[pre-workshop survey](https://forms.office.com/e/XJPJUTn0mp)
prior to the start of the workshop.
This information is vital for us to keep improving the lesson
for other learners.

## Installing Git

::: tab

### Met Office Linux

No setup is required, Git is installed for you.

```bash
$ git --version
git version 2.47.0
```

### Met Office Windows

Please ensure Git is installed via the Company Portal.
You should now have access to the **Git Bash** app.

```bash
$ git --version
git version 2.47.1.windows.1
```

### Non-Met Office Systems

Your institution may have already installed Git on your work computer.
Ask your instructor if this step is necessary or simply type:

```bash
$ git --version
git version 2.47.0
```

If a version number is printed like the output above Git is installed and 
ready to use.
Otherwise please follow the instructions in the
[Git book](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
for your operating system.

:::

## Creating a GitHub Account

You will need an account for [GitHub](https://github.com) to follow
the GitHub episodes of this lesson.

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
[multi-factor authentication (MFA)](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication) 
configured for extra security.
Several options exist for setting up MFA, which are summarised here:

1. If you already use an authenticator app, 
   like [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=en&co=GENIE.Platform%3DiOS&oco=0) 
   or [Duo Mobile](https://duo.com/product/multi-factor-authentication-mfa/duo-mobile-app) on your smartphone for example, 
   [add GitHub to that app](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-mobile-app).
2. If you have access to a smartphone but do not already use an authenticator app, install one and 
   [add GitHub to the app](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-mobile-app).
3. If you do not have access to a smartphone or do not want to install an authenticator app, you have two options:
    1. [set up MFA via text message](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-text-messages) 
       ([list of countries where authentication by SMS is supported](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/countries-where-sms-authentication-is-supported)), or
    2. [use a hardware security key](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-security-key) 
       like [YubiKey](https://www.yubico.com/products/yubikey-5-overview/) 
       or the [Google Titan key](https://store.google.com/us/product/titan_security_key?hl=en-US&pli=1).

The GitHub documentation provides [more details about configuring MFA](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication).

### Passkeys

To completely avoid having authentication for work purposes on a personal device you may choose to [set up a passkey](https://docs.github.com/en/authentication/authenticating-with-a-passkey/managing-your-passkeys). Your instructor and organisation will be able to provide guidance on suitable passkey providers and password managers.

[workshop-setup]: https://carpentries.github.io/workshop-template/install_instructions/#git

## Optional: Git Autocomplete

Git provides a script which lets us display the version control status in your 
terminal prompt.
The following instructions have been tested on **Linux**.
If you are using MacOS or Windows please consult the
[Git autocomplete instructions](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh)
at the top of the linked file and email the support mailbox,
[Science Git Migration Project](mailto:ScienceGitMigrationProjectSupport@metoffice.gov.uk),
if you require help.
To enable this script add the following to a new `~/.bashrc.d/prompt.bash` 
file:

```bash
$ mkdir -p ~/.bashrc.d
$ nano ~/.bashrc.d/prompt.bash
$ cat ~/.bashrc.d/prompt.bash
```

```bash
if [[ $- =~ i ]]; then
    GIT_PROMPT_PATH=/usr/share/git-core/contrib/completion/git-prompt.sh
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

    export PS1='[\u@\h:\w]$(__git_ps1 "(%s)"):\$ '

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

::: caution

### GIT_PROMPT_PATH

The path in the output above is correct for Met Office systems.
If you are not using Met Office systems please consult
your institutions IT services or download your own copy of the `git-prompt.sh` script.
Download the latest version from the 
[Git repository contrib directory](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh).
Ensure the `GIT_PROMPT_PATH` matches where you decide to store the `git-prompt.sh` file.

:::

To see the changes to your terminal prompt run:

```bash
source ~/.bashrc
```

You might not notice much of a change
until you are in a directory containing a Git repository.

::: spoiler

### If you have already modified your prompt

If your `~/.bashrc` file, or any file in the `~/.bashrc.d/` directory,
already modifies your prompt using the `PS1` command you can
`export PROMPT_COMMAND` instead or seek help from your instructor
on how to merge your current `PS1` command with the one above.

Replace the `export PS1` line with:

```bash
export PROMPT_COMMAND=("${PROMPT_COMMAND[@]}" '__git_ps1 "${CONDA_PROMPT_MODIFIER}[\u@\h:\w]:" "\$ " "(%s)"')
```

If your version of Bash is less than 5.1 or you are using
MacOS you might need to use:

```bash
export PROMPT_COMMAND=${PROMPT_COMMAND:+"$PROMPT_COMMAND; "}'__git_ps1 "${CONDA_PROMPT_MODIFIER}[\w]:" "\$ " "(%s)"'
```

:::

::: callout

### How to get help

- Attend a Git and GitHub Surgery.
- Email the [support mailbox](mailto:ScienceGitMigrationProjectSupport@metoffice.gov.uk).

Details of surgeries can be found on the
Science Git Migration Project Comms Site.

:::

### Long Terminal Prompts

You might find that with long paths and usernames your prompt takes up the
entire width of the terminal; there are several ways to reduce the prompt length:

#### Removing `\u` and `\h`

If adding in your username, `\u`, and hostname, `\h`, makes the terminal prompt
too long you can remove the `\u` and or `\h` from the `PROMPT_COMMAND` or
`PS1` lines.

#### Trim long directory paths

Just before the final `fi` line you may add:

```bash
PROMPT_DIRTRIM=3
```

This trims long directory paths to only show the current and two parent directories.
You can change this value to show more or fewer directories.

```output
/Desktop/A/Really/Long/Path $ # without PROMPT_DIRTRIM
.../Really/Long/Path $        # with PROMPT_DIRTRIM
```

#### Add in a newline

Just before the `\$` symbol in the `PS1` or `PROMPT_COMMAND`
lines you may add `\n`.
This will add in a newline before the `$` symbol,
separating your prompt from your terminal commands.

Before:

```output
[~/Documents/git-novice]:(branch_name) $ _
```

After:

```output
[~/Documents/git-novice]:(branch_name)
$ _
```

To see the changes to your terminal prompt run:

```bash
source ~/.bashrc
```
