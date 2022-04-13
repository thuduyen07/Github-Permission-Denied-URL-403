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
2. Check if the repo create using private ssh key
3. Check if the remote url is correct: 
    - show remote url: `git remote -v` 
    - remove exist remote url: `git remote remove <remote-name>`
    - change remote url: `git remote set-url <remote-name> <remote-url>`
  
***Note:** here I change from https to ssh*
4. Check if you're connecting to the correct server: `ssh -vT git@github.com`
    - it should be port 22
    ***Note:** here I got a message as *
        - debug1: No more authentication methods to try.
        - git@github.com: Permission denied (publickey).
  
--- In Git bash ---
  
5. Check if you have a used key: `eval $(ssh-agent -s)`
  - it should show: Agent pid <code>
  
  ***Note:** here I got this message but still can't push code*
6. Generate new ed25519 key: 
    - `ssh-keygen -t ed25519 -C "user-email>"`
    - enter required info ---***Note:**here I just enter for default*
    - Back to Step 5. to check the used key again (here the <code> is changed)
7. Add identity: `ssh-add ~/.ssh/id_ed25519`
    - it should be added succesfully
8. Check connection: `ssh -T git@github.com`
   - it should show: "Hi <user-name>! You've successfully authenticated..."
9. Add SSH keys on Github:
    - In Git bash: `clip < ~/.ssh/id_ed25519` to copy the generated private ssh key
    - On Github browser:
      - Click on profile icon on top right corner 
      - Click on Settings
      - Click on SSH and GPG keys on the left side menu
      - Click on Add SSH key on top right corner
      - Add Title and Key (which is copied from git bash and starts with "ssh-ed25519")
      - Click Add SSH key
                                          
Now you can push code to Github =)) hurayyyyy
