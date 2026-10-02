# VServer
Create a test Virtual Server for learning purposes (loom intro link https://www.loom.com/share/3f5aaa8fc3f2492cb031a0f775497776)

## Setup and copy ssh keys

Step 1 - Generate an SSH key pair. In the terminal type the following: 
```bash
ssh-keygen -t ed25519 -f C:/Users/user-directory/.ssh/id_ed25519_VServer -C "key name comment"
```
> [!Note]
> -t ed25519 : This will generate a new SSH key pair using Ed25519 key type (recommended as it is faster, shorter and has better security properties).
> -f ~/.ssh/id_ed25519_VServer : Specifies where the key pair should be generated (filename, usefull for organization purposes and for multiple key pairs)
> -C "key name comment" : provides a comment for the key pair

Step 2 - You can view your key pairs using the foloowing command:
```bash
    ls ~/.ssh
```
> [!Note]
> ~ represents your home directory

 Step 3 - Connect to the server using the following command:
 ```bash
    ssh user@ip-address   
```

Step 4 - If you are able to connect to the server then you can log off and move on to the next step.
```bash
    logout
```

Step 5 - Copy your ssh public key to the authorized_key file using the following command:
```bash
    ssh-copy-id -i C:/Users/user-directory/.ssh/id_ed25519_VServer.pub user@ip-address
```
> [!Note]
> -i stands for identity

Step 6 - Test your connection using the SSH key:
```bash
    ssh -i C:/Users/user-directory/.ssh/id_ed25519_VServer user@ip-address
```

## Disable Password logins

Step 1 - Enter the config file using the following command:
```bash
    sudo nano etc/ssh/sshd_config
```

Step 2 - Find and edit the line "#PasswordAuthentication yes" to "PasswordAuthentication no"

Step 3 - Save ('Ctrl + O') and exit ('Ctrl + X') the file before restarting the sshd service to reload the config changes.
To restart the service use the command:
```bash
    sudo systemctl restart ssh.service
```

Step 4 - Logout and attemtpt to login with user name and password. If all went well you should receive a Permission denied (publickey) messgae which tells you that you need to use your public key to login

##Setup Nginx

Step 1 - Update the system
    Update the server to prepare for the webserver installation:
```bash
        sudo apt update
```

Step 2 - Install Nginx
    Install the Nginx webserver using the following command:
```bash
sudo apt install nginx -y
```
> [!Note]
> -y (yes) confirms the installation


Step 3 - Verify Nginx status
To check if Nginx is running use the following command:
```bash
systemctl status nginx.service
```