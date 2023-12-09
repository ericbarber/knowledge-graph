# SSH Key Gen
Generating an SSH Key on Your Computer

## Open Your Terminal:
* On macOS or Linux, open the Terminal application.
* On Windows, use Git Bash or another terminal that supports SSH.

## Generate SSH Key:
Run the command:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
* Replace "your_email@example.com" with your GitHub email. 

The ed25519 algorithm is recommended for security, but if it's not supported, use:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

## Save the SSH Key:
When prompted:
```bash
"Enter a file in which to save the key,"
```
press Enter to accept the default file location (~/.ssh/id_ed25519 or ~/.ssh/id_rsa).
Enter a secure passphrase when prompted (optional, but recommended for additional security).

## Start the SSH Agent:
Run:
```bash
eval "$(ssh-agent -s)"
```
 to start the SSH agent in the background.

## Add SSH Key to the SSH Agent:
* For macOS or Linux, use
```bash
ssh-add ~/.ssh/id_ed25519
```
(or id_rsa if you used RSA).
* For Windows, the SSH agent might work differently depending on the environment.

## Adding the SSH Key to GitHub

### Copy Your SSH Public Key:
To display and copy your public key to your clipboard, on Windows use 
```bash
cat ~/.ssh/id_ed25519.pub | clip
```
,or cat ~/.ssh/id_ed25519.pub | pbcopy on macOS. 
On Linux, you might need to manually copy the output after running 
```bash
cat ~/.ssh/id_ed25519.pub.
```

### Add Key to GitHub Account:
* Go to GitHub and sign in.
* Click your profile photo, then click Settings.
* In the user settings sidebar, click SSH and GPG keys.
* Click New SSH key or Add SSH key.
* In the "Title" field, add a descriptive label for the new key (e.g., "My Laptop").
* Paste your key into the "Key" field (it should start with ssh-ed25519 or ssh-rsa).
* Click Add SSH key.

## Test Your SSH Connection:
* Run:
```bash
ssh -T git@github.com
```
to test your connection.
If prompted, verify the authenticity of the GitHub host and type "yes".
If everything is set up correctly, you should see a message like "Hi [username]! You've successfully authenticated, but GitHub does not provide shell access."

Now, you should be able to use SSH for Git operations with GitHub on your computer. Remember, the private key (id_ed25519 or id_rsa) should always be kept secure and private.

# Maintain Multiple SSH Keys
Managing multiple SSH keys for different GitHub accounts from the same computer is quite common, especially for developers who need to separate personal and work-related repositories. Here’s how you can set this up efficiently:
Step 1: Generate Multiple SSH Keys

First, ensure you have a unique SSH key for each account. If you haven't already, generate them as described previously, but save each key with a unique name. For example:

    For your personal account: ssh-keygen -t ed25519 -C "personal_email@example.com" -f ~/.ssh/id_ed25519_personal
    For your company account: ssh-keygen -t ed25519 -C "work_email@example.com" -f ~/.ssh/id_ed25519_work
    For other purposes: ssh-keygen -t ed25519 -C "other_email@example.com" -f ~/.ssh/id_ed25519_other

Step 2: Add SSH Keys to the SSH Agent

After generating the keys, add them to your SSH agent:

    Run eval "$(ssh-agent -s)" to start the agent.
    Add each key using ssh-add ~/.ssh/id_ed25519_personal, ssh-add ~/.ssh/id_ed25519_work, etc.

Step 3: Create a SSH Config File

This step is crucial for managing multiple keys. You’ll create a SSH config file to define which key should be used for which domain.

    Open or create the SSH config file in your text editor: ~/.ssh/config.

    Add configuration for each account. Here’s an example:

    ```bash
    # Personal account
    Host github.com-personal
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_personal

    # Work account
    Host github.com-work
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_work

    # Other account
    Host github.com-other
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_other
    ```

    In this configuration, Host is a nickname for each connection (it can be anything), HostName is the actual server, User is always git for GitHub, and IdentityFile specifies which private key to use.

Step 4: Add SSH Keys to GitHub Accounts

Add each public key to the respective GitHub account as described in the previous steps.
Step 5: Clone and Use Repositories

When you clone a repository, use the Host nickname from your SSH config file. For example:

    For a personal repository: git clone git@github.com-personal:username/repo.git
    For a work repository: git clone git@github.com-work:username/repo.git

This tells Git which SSH configuration (and therefore, which key) to use for each operation.
Step 6: Handling Existing Repositories

If you already have repositories cloned with a single account, you can change the repository's remote URL to match the new config:

    Change remote URL with git remote set-url origin git@github.com-personal:username/repo.git (adjust accordingly).

Tips

    Ensure your SSH agent is running and keys are added every time you start a new session.
    Regularly verify and update your SSH keys for security purposes.
    Keep your private keys secure and never share them.

By following these steps, you can smoothly manage multiple GitHub accounts and their respective SSH keys on a single computer.

