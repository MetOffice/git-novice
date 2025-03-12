---
title: Setup
---

## How to get help

- Attend a Git and GitHub Surgery.
- Email the [support mailbox](mailto:ScienceGitMigrationProjectSupport@metoffice.gov.uk).

Details of surgeries can be found on the
Science Git Migration Project Comms Site.

## Lesson Outcomes

This lesson is split into two parts.
Episodes in the first half before the break focus on Git.
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
[pre-lesson survey](https://forms.office.com/e/XJPJUTn0mp)
prior to the start of the lesson.
This information is vital for us to keep improving the lesson
for other learners.

## Installing Git

::: tab

### Met Office Linux

No setup is required, Git is installed for you.
Your version number may be different to the example below.

```bash
$ git --version
git version 2.47.0
```

### Met Office Windows

Please ensure Git is installed via the Company Portal.
You should now have access to the **Git Bash** app.
Your version number may be different to the example below.

```bash
$ git --version
git version 2.47.1.windows.1
```

### Non-Met Office Systems

The systems at your institution may already have Git installed.
You can check this by typing:

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

To completely avoid having authentication for work purposes on a personal device you may choose to [set up a passkey](https://docs.github.com/en/authentication/authenticating-with-a-passkey/managing-your-passkeys). Your instructor or organisation will be able to provide guidance on suitable passkey providers and password managers.
At the Met Office the KeePass password manager is available.
Search, "Using KeePass for one-time passwords" on SharePoint for
setup instructions.

## SSH Setup

::: callout

### What if I already have an SSH key?

Run:

```bash
ssh -T git@github.com
```

If you see the following output please skip this setup section:

```output
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

:::

::: spoiler

### Note for Personal Access Token Users

We recommend you move to using SSH Keys
instead of a PAT (instructions below).
This material will work using a PAT,
please see the **Note for Personal Access Token Users** 
dropdown in the [first GitHub episode](../episodes/07-github.md).

:::

Before you can connect to a repository on GitHub,
you need to set up a way for your computer to authenticate with GitHub.

We are going to set up the method that is commonly used by many different services to authenticate access on the command line.
This method is called Secure Shell Protocol (SSH).
SSH is a cryptographic network protocol that allows secure communication between computers using an otherwise insecure network.

SSH uses what is called a key pair.
This is two keys that work together to validate access.
One key is publicly known and called the public key,
and the other key called the private key is kept private.

You can think of the public key as a padlock,
and only you have the key (the private key) to open it.
You use the public key where you want a secure method of communication,
such as your GitHub account.
You give this padlock, or public key,
to GitHub and say "lock the communications to my account with this so that only computers that have my private key can unlock communications and send git commands as my GitHub account."

What we will do now is the minimum required to set up an SSH key
and add the public key to a GitHub account.

:::::::::::::::::::::::::::::::::::::::::  callout

## Keeping your keys secure

You shouldn't really forget about your SSH keys, since they keep your account secure.
It's good practice to audit your secure shell keys every so often.
Especially if you are using multiple computers to access your account.


::::::::::::::::::::::::::::::::::::::::::::::::::

Run the list command to check what key pairs already exist on your computer.

```bash
ls -al ~/.ssh
```

Your output is going to look a little different depending on whether or not SSH has ever been set up on the computer you are using.

If you have not set up SSH on your computer, you will see

```output
ls: cannot access '~/.ssh': No such file or directory
```

If SSH has been set up on the computer you're using, the public and private key pairs will be listed. The file names are either `id_ed25519`/`id_ed25519.pub` or `id_rsa`/`id_rsa.pub` depending on how the key pairs were set up.

### Create an SSH key pair

To create an SSH key pair use the following command, 
where the `-t` option specifies which type of algorithm to use:

::: group-tab

### Linux and MacOS

```bash
$ ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_github -C "e.ormerod@mo-weather.uk Azure SPICE"
```

### Windows

In Git bash run:

```bash
$ ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_github -C "e.ormerod@mo-weather.uk Azure SPICE"
```

If this command fails:

Replace the `ssh-keygen` command with `ssh-keygen.exe`.

Or:

1. Open the Windows command prompt by searching for **cmd** in the search bar or start menu.
2. Re-run the command above in the command prompt.

:::

The `-f` flag specifies a path to a file to store the key in.

The `-C` flag attaches a comment to the key.
The comment has no effect on your key, you may place anything here to help
you remember what the key is for. It makes no difference whether you use
a public email or your no-reply private GitHub email in the comment.

If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
`$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_ed25519_github -C "your_email@example.com"`

```output
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase):
```

Now you will be prompted for a passphrase.
If the computer you work on is shared between multiple users you should 
set a passphrase. 
Be sure to use something memorable or save your passphrase somewhere,
as there is no "reset my password" option.
If you do not share your computer there is no need 
to set a passphrase so just press <kbd>Enter</kbd>.

Note that, when typing a passphrase on a terminal, there won't be any visual feedback of your typing.
This is normal: your passphrase will be recorded even if you see nothing changing on your screen.

```output
Enter same passphrase again:
```

After entering the same passphrase a second time, we receive the confirmation

```output
Your identification has been saved in ~/.ssh/id_ed25519_github
Your public key has been saved in ~/.ssh/id_ed25519_github.pub
The key fingerprint is:
SHA256:SMSPIStNyA00KPxuYu94KpZgRAYjgt9g4BA4kFy3g1o e.ormerod@mo-weather.uk Azure SPICE
The key's randomart image is:
+--[ED25519 256]--+
|^B== o.          |
|%*=.*.+          |
|+=.E =.+         |
| .=.+.o..        |
|....  . S        |
|.+ o             |
|+ =              |
|.o.o             |
|oo+.             |
+----[SHA256]-----+
```

The "identification" is actually the private key. You should never share it.
The public key is appropriately named.
The "key fingerprint" is a shorter version of a public key.

Now that we have generated the SSH keys, we will find the SSH files when we check.

```bash
ls -al ~/.ssh
```

```output
drwxr-xr-x 1 Eleanor   197121   0 Jul 16 14:48 ./
drwxr-xr-x 1 Eleanor   197121   0 Jul 16 14:48 ../
-rw-r--r-- 1 Eleanor   197121 419 Jul 16 14:48 id_ed25519_github
-rw-r--r-- 1 Eleanor   197121 106 Jul 16 14:48 id_ed25519_github.pub
```

### Copy the public key to GitHub

Now we have an SSH key pair and we can run this command to check if GitHub can read our authentication.

```bash
ssh -T git@github.com
```

```output
The authenticity of host 'github.com (192.30.255.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
```

Now we need to give GitHub the public key.

::: spoiler

### Checking the GitHub RSA Key

Ideally before connecting to a new host, like `github.com` in the output above,
you would check the RSA key fingerprint matches the expected value.
GitHub publishes their public [SSH key fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)
for you to check against.

:::

First, we need to copy the public key. 
Be sure to include the `.pub` at the end, otherwise you're looking at the private key.

```bash
cat ~/.ssh/id_ed25519_github.pub
```

```output
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDmRA3d51X0uu9wXek559gfn6UFNF69yZjChyBIU2qKI e.ormerod@mo-weather.uk
```

Now, go to [GitHub.com](www.github.com), click on your profile icon in the top right corner to get the drop-down menu.
Click "Settings", then on the settings page, click "SSH and GPG keys",
on the left side "Access" menu.
Click the "New SSH key" button on the right side.
Now, you can add the title (normally an ID for the computer storing the keys such as "Work Linux"),
paste your SSH key into the field, and click the "Add SSH key" to complete the setup.

::: callout

## Single sign-on (SSO)

If you are part of an organisation that requires single
sign-on (SSO) to access their GitHub organisation you
will need to authorise the key for use in the organisation.

Next to the newly created SSH key in the GitHub settings
click on "Configure SSO".
Find the organisation in the list and click on "Authorise".

:::

Now check your authentication again from the command line.

```bash
$ ssh -T git@github.com
```

```output
Hi Eleanor! You've successfully authenticated, but GitHub does not provide shell access.
```

This output confirms that the SSH key works as intended.

::: spoiler

### Troubleshooting SSH Setup

If your new key failed to connect you may need to alter your ssh config.

1. Create the `~/.ssh/config` file if it doesn't exist
2. Add the following to the file:

```ssh-config
Host github.com
  IdentityFile ~/.ssh/id_ed25519_github
```

This explicitly states which key to use for `github.com`
and is needed if you have many SSH keys already for other hosts.

:::

## Optional: Git Autocomplete

Git provides a script which lets us display the version control status in your 
terminal prompt.
The following instructions have been tested on **Linux**.
If you are using MacOS or Windows please consult the
[Git autocomplete instructions](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh)
at the top of the linked file and
[reach out for support](./learners/setup.md#how-to-get-help) if you require help.
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
        . "${GIT_PROMPT_PATH}" >&2
        export GIT_PS1_SHOWDIRTYSTATE=1 # this can potentially slow down the prompt
        export GIT_PS1_SHOWSTASHSTATE=1
        export GIT_PS1_SHOWUPSTREAM="auto"
        export GIT_PS1_SHOWCOLORHINTS=1
        export GIT_PS1_SHOWUNTRACKEDFILES=1 # this can potentially slow down the prompt

        export PS1='[\u@\h:\w]$(__git_ps1 "(%s)"):\$ ' # style to your taste
    #else # optional, if you need to style the default prompt without Git 
    #    export PS1='[\u@\h:\w] \$ '
    fi
fi
```

You may use your preferred editor to create this file;
these lesson materials use the [`nano`](https://www.nano-editor.org/) editor
when a file needs creating or modifying.
This is followed by the `cat` command in the material
to show the file contents after the change.
You do not have to use the `cat` command when following
the lesson material.

Make sure your `~/.bashrc` file includes:

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

These lines should only be added to the `~/.bashrc` file. Do not add them to your `prompt.bash` file.

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
`export PROMPT_COMMAND` instead.

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

::: spoiler

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

:::
