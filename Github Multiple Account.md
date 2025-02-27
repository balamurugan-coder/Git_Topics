# Add Multiple Git Accounts in Local Machine Using SSH

## 1. Set up SSH keys for each account

- Open `GitBash` and make sure that you are in the base directory `C:/Users/Username/`
- Type `ls -la` to check if the .ssh folder exists in the directory. If it does, proceed with the following commands to generate SSH keys.

```bash
# For the first Git account
ssh-keygen -t rsa -b 4096 -C "your-email1@example.com" -f ".ssh/id_rsa_account1"
```

- The above command will generate the private and public keys and store into .ssh folder
- **id_rsa_account1** replace this with your desired name

```bash
# For the second Git account
ssh-keygen -t rsa -b 4096 -C "your-email2@example.com" -f ".ssh/id_rsa_account2"
```

- The above command will generate the private and public keys and store into .ssh folder
- **id_rsa_account2** replace this with your desired name

Now you have pair of keys for both git accounts - Public and Private key
- Public key ends with extenion `id_rsa_account2.pub`

## 2. Create Host Config 
- Create `nano /c/Users/username/.ssh/config` to create host config file for mutliple accounts
- Windows users follow this command `nano c:/Users/username/.ssh/config`

```bash
Host git-account1
    HostName github.com
    User git
    IdentityFile /c/Users/username/.ssh/id_rsa_account1

Host git-account2
    HostName github.com
    User git
    IdentityFile /c/Users/username/.ssh/id_rsa_account2
```
- Save and close the file

## 3. Add Public Key to GitHub Repo
- `cat /c/Users/username/.ssh/id_rsa_account2.pub"` - This will show the key, just copy it
- Login into your GitHub Portal
- Choose your repository that you want to setup access - `username/reponame`
- Go to `Settings` -> Choose `Deploy Keys` and Paste your public key here 
- Make sure to enable the write access checkbox and press save button

## 4. Add Keys to local repo
- In GitBash, check SSH Agent is running by doing `ssh-add -l`.
- If it is not running Run using `eval $(ssh-agent -s)`.
- Add your private key to the current SSH agent session `ssh-add /c/Users/username/.ssh/id_rsa_account2`

**When you close the github the SSH session will be gone, you have to repeat these steps.**

## 5. Add Remote Repo to local Machine
- Open your local repo in GitBash, and do `git config user.name=Your Name` and `git config user.email=your-email2@example.com`
- run `git add remote AnyName git@git-account2:username/reponame.git`
- Now you check the remote list by running `git remote -v`
- You should see the fetch and pull remote should be added like above
```bash
git remote -v
YourName     git@git-account2:username/reponame.git (fetch)
YourName     git@git-account2:username/reponame.git (push)
```
- Now you can push and pull from remote repository with different git account
- `git push -u YourName main`

Enjoy :)
