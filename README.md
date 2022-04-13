# Github permission denied - requested URL 403 error

## Questions
1. Why can't we use https for pushing this repo?
2. How can we create a github repo such this? 

## Description
- This is the guide for solving the issue:
  - remote: Permission to <repo> denied to <user>
  - fatal: unable to access <repo-ssh>: The requested URL returned error: 403
- This issue happens when I push code to the Github repo. Note: I'm already in the collaborators list.

## Step to solve:
--- On Github browser --- 

1. Check if you're already in the collaborators list 
    - if not, please follow these steps:
      1. Go to the repo on Github
      2. Click on Settings tab
      3. Click on "Collaborators" tab on the left side menu
      4. Add the Github account or email of collaborator
      5. Click Add
  
--- In IDE ---  

2. Check if the repo create using private ssh key <br/>

3. Check if the remote url is correct: 
    - show remote url: `git remote -v` 
    - remove exist remote url: `git remote remove <remote-name>`
    - change remote url: `git remote set-url <remote-name> <remote-url>`
  
   ***Note:** here I change from https to ssh*

4. Check if you're connecting to the correct server: `ssh -vT git@github.com`
    - it should be port 22

    - ***Note:** here I got a message as* 
        - debug1: No more authentication methods to try.
        - git@github.com: Permission denied (publickey).
  
--- In Git bash ---
  
5. Check if you have a used key: `eval $(ssh-agent -s)`
    - it should show: `Agent pid <code>`
  
    ***Note:** here I got this message but still can't push code*

6. Generate new ed25519 key: 
    1. `ssh-keygen -t ed25519 -C "user-email>"`
    2. enter required info 
        - ***Note:** here I just enter for default*
    3. Back to Step 5. to check the used key again (here the `<code>` is changed)

7. Add identity: `ssh-add ~/.ssh/id_ed25519`
    - it should be added succesfully

8. Check connection: `ssh -T git@github.com`
   - it should show: "Hi <user-name>! You've successfully authenticated..."
9. Add SSH keys on Github:
    1. In Git bash: `clip < ~/.ssh/id_ed25519` to copy the generated private ssh key
    2. On Github browser:
        1. Click on profile icon on top right corner 
        2. Click on Settings
        3. Click on SSH and GPG keys on the left side menu
        4. Click on Add SSH key on top right corner
        5. Add Title and Key (which is copied from git bash and starts with "ssh-ed25519")
        6. Click Add SSH key
                                          
Now you can push code to Github =)) hurayyyyy

## Reference
1. [PR issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)
2. [Remove remote url](https://www.delftstack.com/howto/git/how-to-remove-a-git-remote-url/)
3. [The requested URL returned error: 403](https://namespaceit.com/blog/fatal-unable-to-access-the-requested-url-returned-error-403#:~:text=While%20working%20with%20Github%20you%20may%20face%20that%2C,repository%20has%20been%20disabled.%20The%20error%20looks%20like%2C)
4. [Managing remote repositories](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories)
5. [Testing your SSH connection](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)
6. [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
7. [Error: Permission denied (publickey)](https://docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey#check-that-you-are-connecting-to-the-correct-server)
