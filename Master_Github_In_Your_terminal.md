# Master GitHub In Your Terminal

### Table of Contents:
- [Creating A SSH Key](#creating-a-ssh-key)
- [Creating A Token](#creating-a-token)
- [Cloning A Repository](#cloning-a-repository)
- [Setting Your Git Username And Email](#setting-your-git-username-and-email)
- [Adding A Remote Repository](#adding-a-remote-repository)
- [Making A New Branch](#making-a-new-branch)
- [How To Pull From A Branch](#how-to-pull-from-a-branch)
- [How To Push To A Branch](#how-to-push-to-a-branch)
- [Opening Jaiabot Repository And Branch From Terminal](#opening-jaiabot-repository-and-branch-from-terminal)
- [Viewing Commit History](#viewing-commit-history)
- [Checking The Status Of Your Git Repository](#checking-the-status-of-your-git-repository)
- [How To Merge Two Branches](#how-to-merge-two-branches)
- [How To Fix Merge Conflict](#how-to-fix-merge-conflict)
- [Undoing Changes](#undoing-changes)
- [Redoing Changes](#redoing-changes)

<br>
<a id="creating-a-ssh-key"></a>

# Creating A SSH Key
1. go to GitHub and log in as your user
2. Click on avatar
3. Click on Settings
4. in settings under access click on SSH and GPG keys
5. click New SSH key
6. Give SSH key a title
7. Have key type be
```
Authentication Key
```

8. Then in terminal enter these commands
```
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```
9. Copy key should look something like this
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIM3NGaqFmDpDATu/AP4uVi/2KBHYlI14A1iptVF3AbwG your_email@example.com
```
10. Then in GitHub past key and click Add SSH key
11. In terminal enter command 
```
ssh -T git@github.com
# If key was created correctly the output should be Hi GitHub_username! You've successfully authenticated, but GitHub does not provide shell access.
```

<br>
<a id="creating-a-token"></a>

# Creating A Token
1. go to GitHub and log in as your user
2. Click on avatar
3. Click on settings
4. Click on Developer Settings
5. Click on Personal access Tokens
6. Click on Tokens (classic)
7. Click on Generate new token classic
8. Name token
9. Click on all boxes
10. Copy token save to a safe place because otherwise it will be lost

<br>
<a id="cloning-a-repository"></a>

# Cloning A Reposoitory
1. Clone a repo from terminal
```
git clone https://github.com/UserName/nameOfRepo
```

<br>
<a id="setting-your-git-username-and-email"></a>

# Setting Your Git Username And Email
1. Set your global name and email 
```
# This applies to all repos
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```
2. If you've already made a commit and want to fix author info
```
git commit --amend --reset-author
```

<br>
<a id="adding-a-remote-repository"></a>

# Adding A Remote Repository 
Note: Add a remote repository (if not already set) in order to be able to pull and push in terminal
1. If no remote is set yet, add the URL of the GitHub repository you want to connect to
```
git remote add origin git@github.com:YourUserName/YourRepoName.git
```
2. If remote already exists but you want to update the URL, then set the remote URL
```
git remote set-url origin git@github.com:YourUserName/YourRepoName.git
# Note: jaiabot repo would require this command
git remote set-url origin git@github.com:jaiarobotics/jaiabot.git
```
3. Verify the remote is correctly set
```
git remote -v
# The expected output should look like this
origin  git@github.com:YourUserName/YourRepoName.git (fetch)
origin  git@github.com:YourUserName/YourRepoName.git (push)
```

<br>
<a id="making-a-new-branch"></a>

# Making A New Branch
**Create a new branch off of 1.y**
```
git checkout 1.y
git pull
# make sure to follow naming conventions when creating a new branch
git checkout -b branch name
```
**If You want to switch branches**
```
# All changes must be pushed before your allowed to switch branches
git checkout branchName
```

<br>
<a id="how-to-pull-from-a-branch"></a>

# How To Pull From A Branch
Note: If you want to list all branches
```
git branch -r
q #To quit
```
1. Switch to branch you want to pull from
```
git checkout branchName
```
2. Pull all new changes from branch
```
git pull origin branchName
```
3. Enter Visual Studio Code 
```
code .
```

<br>
<a id="how-to-push-to-a-branch"></a>

# How To Push To A Branch
Note: If you want to list all branches
```
git branch -r
q #To quit
```
1. Switch to branch you want to push to
```
git checkout branchName
```
2. Add your changes
```
git add .
```
3. Commit your changes
```
git commit -m "message"
```
4. Push your changes to the GitHub
```
git push origin branchName
```

<br>
<a id="opening-jaiabot-repository-and-branch-from-terminal"></a>

# Opening Jaiabot Repository And Branch From Terminal
Note: Must clone jaiabot repository before opening it in your browser from terminal.
1. To open the main repository page
```
xdg-open https://github.com/jaiarobotics/jaiabot
```
2. To open a specific branch in jaiabot repository
```
xdg-open https://github.com/jaiarobotics/jaiabot/tree/branchName
```

<br>
<a id="viewing-commit-history"></a>

# Viewing Commit History
1. To see commit log
```
git log
```
2. To view commit log in a single line per commit
```
git log --oneline
```
3. Press q to quit

<br>
<a id="checking-the-status-of-your-git-repository"></a>

# Checking The Status Of Your Git Repository
Note: Before making any changes, it is useful to check the status of your Git repository. This will show you what files have been modified, added, or deleted. This keeps track of all changes and ensures you're not missing anything.
```
git status
```
**This will show**
* Files that have been modified but not staged
* Files that have been staged and ready to be committed
* Any files not added to Git

<br>
<a id="how-to-merge-two-branches"></a>

# How To Merge Two Branches
1. Check your current branch
```
git branch
```
2. Switch to the branch you want to merge
```
git checkout branchName
```
3. Pull the Latest changes if needed
```
git pull origin branchName
```
4. Merge the other branch
```
git merge branchName
```
5. Push merge to the repository
```
git add .
git commit -m "message"
git push origin branchName
```

<br>
<a id="how-to-fix-merge-conflict"></a>

# How To Fix Merge Conflict
Note: When merge conflict happens your file like have conflict markers and you need to manually resolve these conflicts by editing the file to choose which changes to keep. After you must add this file again.
1. Open file with merge conflict you should see conflict markers
2. Decide what changes to keep
* Review both sets of changes and decide which to keep, or manually combine both changes
* Remove the conflict markers, keeping only the changes, you want.
3. Then push the changes 
```
# you can stage all modified files by git add . or just the modified file by git add <file-name>
git add . 
git commit -m "Resolved merge conflict"
git push origin branchName
```
<br>
<a id="undoing-changes"></a>

# Undoing Changes
1. Check commit log to choose the commit you want to revert to
```
git log
```
2. Copy the commit ID
```
# Commit id should look like this
commit b17ff34240c09c008ac63c997a18e721273838b0
```
3. Reset to that commit
```
git reset --hard b17ff34240c09c008ac63c997a18e721273838b0
```
4. Force push the reset
```
git push origin HEAD --force
```
5. verify changes were undone to the selected commit

<br>
<a id="redoing-changes"></a>

# Redoing Changes
1. Show the reflog to view previous HEAD positions, including the most recent commit before the reset
```
git reflog
```
2. Copy the short hash of the commit you want to revert back to
```
# Commit hash should look something like this
0dd5856
```
3. Reset to that commit 
```
git reset --hard commitHash
```
4. Force push the reset
```
git push origin HEAD --force
```
5. Verify that the changes have been restored