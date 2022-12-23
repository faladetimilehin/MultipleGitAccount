# MultipleGitAccount

# Introduction

Let’s imagine a scenario: you’ve just joined a new company, it’s your first day and you need to set up your new machine. The first thing you do is to rush to get your own GitHub SSH key and install it so you can still work on your hobby project.
A bit later that day, you are finally able to login to your organization repository and you now need to install this key as well to be able to start working.
As a developer working to set up his Git config at least once. Our cheat sheet will help you make this process a breeze,
ensuring that you never push with the wrong profile again!
However, SSH has several key benefits over HTTPS authentication, including:

- **No need to continually authenticate.**
  HTTPS often requires you to input your password when switching accounts
- **Limited account access.**
  If your SSH key is compromised, the attacker will not have full access to your git account. And you can easily revoke the SSH key without having to reset all your passwords
- **Expiry**. You can set expiry dates on the SSH keys so they are only active for a certain amount of time. For example, setting your SSH key to only be active for the time you are on a project

- Multiple git profiles can be seamlessly managed on the same machine via a config file
  Taking the time to understand and use SSH keys will make your life easier in the long run. SSH keys are useful not just for git authentication but also for accessing virtual machines on cloud providers etc..

# How To Work With Multiple Github Accounts on a single Machine

Let suppose I have two github accounts, **https:/<span></span>/github.com<span></span>/faladetimilehin** and **https:/<span></span>/github.com<span></span>/tfalade**. Now i want to setup my mac to easily talk to both the github accounts.

> NOTE: This logic can be extended to more than two accounts also. :)

The setup can be done in 5 easy steps:

## Steps:

- [Step 1](#step-1) : Create SSH keys for all accounts
- [Step 2](#step-2) : Add SSH keys to SSH Agent
- [Step 3](#step-3) : Add SSH public key to the Github
- [Step 4](#step-4) : Create a Config File and Make Host Entries
- [Step 5](#step-5) : Cloning GitHub repositories using different accounts

<br>

## Step 1

### Create SSH keys for all accounts

First make sure your current directory is your **.ssh** folder.

```sh
     $ cd ~/.ssh
```

Syntax for generating unique ssh key for ann account is:

```sh
     ssh-keygen -t rsa -C "your-email-address" -f "github-username"
```

here,

**-C** stands for comment to help identify your ssh key

**-f** stands for the file name where your ssh key get saved

#### Now generating SSH keys for my two accounts

```sh
     ssh-keygen -t rsa -C "office_email@gmail.com" -f "github-tfalade"
     ssh-keygen -t rsa -C "personal_email@gmail.com" -f "github-faladetimilehin"
```

Notice here **tfalade** and **faladetimilehin** are the username of my github accounts corresponding to **office_email<span></span>@gmail.com** and **personal_email<span></span>@gmail.com** email ids respectively.

After entering the command the terminal will ask for passphrase, leave it empty and proceed.

![Passphrase Image]

> Now after adding keys , in your .ssh folder, a public key and a private will get generated.

> The public key will have an extention **.pub** and private key will be there without any extention both having same name which you have passed after **-f** option in the above command. (in my case **github-tfalade** and **github-rahu-personal**)

![Added Key Image]

<br>

## Step 2

### Add SSH keys to SSH Agent

Now we have the keys but it cannot be used until we add them to the SSH Agent.

```sh
     ssh-add -K ~/.ssh/github-tfalade
     ssh-add -K ~/.ssh/github-faladetimilehin
```

You can read more about adding keys to SSH Agent [here.](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

<br>

## Step 3

### Add SSH public key to the Github

For the next step we need to add our public key (that we have generated in our previous step) and add it to corresponding github accounts.

For doing this we need to:

**1. Copy the public key**

     We can copy the public key either by opening the github-tfalade.pub file in vim and then copying the content of it.

```sh
     vim ~/.ssh/github-tfalade.pub
     vim ~/.ssh/github-faladetimilehin.pub
```

<p align="center">OR

We can directly copy the content of the public key file in the clipboard.

```sh
     pbcopy < ~/.ssh/github-tfalade.pub
     pbcopy < ~/.ssh/github-faladetimilehin.pub
```

**2. Paste the public key on Github**

- Sign in to Github Account
- Goto **Settings** > **SSH and GPG keys** > **New SSH Key**
- Paste your copied public key and give it a Title of your choice.

**_OR_**

- Sign in to Github
- Paste this link in your browser (https://github.com/settings/keys) or click [here](https://github.com/settings/keys)
- Click on **New SSH Key** and paste your copied key.

<br>

## Step 4

### Create a Config File and Make Host Entries

The **~/.ssh/config** file allows us specify many config options for SSH.

If **config** file not already exists then create one (make sure you are in **~/.ssh** directory)

```sh
     touch config
```

The commands below opens config in your default editor....Likely TextEdit, VS Code.

```sh
     open config
```

Now we need to add these lines to the file, each block corresponding to each account we created earlier.

```config
  # Work GitHub account
  Host github.com-tfalade423
          HostName github.com
          User git
          IdentityFile ~/.ssh/github-tfalade423
  # Personal GitHub account
  Host github.com-faladetimilehin
          HostName github.com
          User git
          IdentityFile ~/.ssh/github-faladetimilehin
```

<br>

## Step 5

### Cloning GitHub repositories using different accounts

So we are done with our setups and now its time to see it in action. We will clone a repository using one of the account we have added.

Make a new project folder where you want to clone your repository and go to that directory from your terminal.

For Example:
I am making a repository on my personal github account and naming it **TestRepo**
Now for cloning the repo use the below command:

```git
    git clone git@github.com-{your-username}:{owner-user-name}/{the-repo-name}.git

    [e.g.] git clone git@github.com-faladetimilehin:faladetimilehin/TestRepo.git
```

 <br>

## Finally

From now on, to ensure that our commits and pushes from each repository on the system uses the correct GitHub user — we will have to configure **user.email** and **user.name** in every repository freshly cloned or existing before.

To do this use the following commands.

```git
     git config user.email "office_email@gmail.com"
     git config user.name "Falade Timilehin"

     git config user.email "personal-email@gmail.com"
     git config user.name "Falade Timilehin"
```

Pick the correct pair for your repository accordingly.

To push or pull to the correct account we need to add the remote origin to the project

```git
     git remote add origin git@github.com-faladetimilehin:faladetimilehin

     git remote add origin git@github.com-tfalade:tfalade
```

Now you can use:

```git
     git push

     git pull
```
