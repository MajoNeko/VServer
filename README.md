# V-Server Setup
Create a test Virtual Server for learning purposes
(loom intro link https://www.loom.com/share/3f5aaa8fc3f2492cb031a0f775497776)

Guide: [PDF Checklist](https://github.com/MajoNeko/VServer/blob/main/Docs/Git_VServer_Checkliste.pdf)

In this walkthrough you will learn how to setup SSH keys on a virtual server, disable password authentication in favour of using SSH authentication, installing and configuring a web server and setting up a Git account with username, email and SSH key on your server.

# Table of Contents

0. [**Introduction**](#V-Server-Setup)
   - Overview of the guide  

1. [**Setup SSH Key Pair**](#Step-1-Generate-an-SSH-key-pair)
   - Create and use a secure SSH key  

2. [**Disable Password Logins**](#Disable-Password-logins)
   - Disable password logins for secure SSH authentication 

3. [**Setup Nginx**](#Setup-Nginx)
   - Install and Configure Nginx

4. [**Setup Git**](#Setup-Git)
   - Install and Configure Git


## Setup and copy SSH keys

### Step 1 - Generate an SSH key pair 
In the terminal type the following: 
```bash
ssh-keygen -t ed25519 -f C:/Users/user-directory/.ssh/id_ed25519_VServer -C "key name comment"
```
> [!Note]
> -t ed25519 : This will generate a new SSH key pair using Ed25519 key type (recommended as it is faster, shorter and has better security properties).

> -f ~/.ssh/id_ed25519_VServer : Specifies where the key pair should be generated (filename, usefull for organization purposes and for multiple key pairs)

> -C "key name comment" : provides a comment for the key pair

### Step 2 : Verify your SSH key pairs 
You can view your key pairs using the foloowing command:
```bash
    ls ~/.ssh
```
> [!Note]
> ~ represents your home directory

### Step 3 - Test your server connection
Connect to the server using the following command:
 ```bash
    ssh user@ip-address   
```

If you are able to connect to the server then you can log off and move on to the next step.
```bash
    logout
```

### Step 4 - Copying your SSH key
Copy your SSH public key to the authorized_key file using the following command:
```bash
    ssh-copy-id -i C:/Users/user-directory/.ssh/id_ed25519_VServer.pub user@ip-address
```
> [!Note]
> -i stands for identity

### Step 6 - Test your connection using the SSH key:
```bash
    ssh -i C:/Users/user-directory/.ssh/id_ed25519_VServer user@ip-address
```

## Disable Password logins

### Step 1 - Open the config file
Open the config file for editing using the following command:
```bash
    sudo nano etc/ssh/sshd_config
```

### Step 2 - Changing Password authentication configuration
Find and edit the line:
```bash 
    "#PasswordAuthentication yes" 
```
change it to
```bash
    "PasswordAuthentication no"
```
Save ('Ctrl + O') and exit ('Ctrl + X') the file

### Step 3 - Restart the service
Restarting the sshd service to reload the config changes.
To restart the service use the command:
```bash
    sudo systemctl restart ssh.service
```

### Step 4 - Test the Configuration
Logout and attempt to login with user name and password. If all went well you should receive a Permission denied (publickey) messgae which tells you that you need to use your public key to login.
You can now securely login using your SSH key as demonstrated in 
[**Setup and copy SSH keys - Step 6**](#Step-6-Test-your-connection-using-the-SSH-key:)

## Setup Nginx

### Step 1 - Update the system
Update the server to prepare for the webserver installation:
```bash
        sudo apt update
```

### Step 2 - Install Nginx
Install the Nginx webserver using the following command:
```bash
sudo apt install nginx -y
```
> [!Note]
> -y (yes) confirms the installation


### Step 3 - Verify Nginx status
To check if Nginx is running use the following command:
```bash
systemctl status nginx.service
```
If you enter your Virtual Servers IP address in the browser you should now see the default Nginx HTML starting page

### Step 4 - Create an alternative starting page
Create a new directory for the alternative HTML page
```bash
    sudo mkdir /var/www/alternatives
```
Create the HTML file:
```bash
    sudo touch /var/www/alternatives/alternate-index.html
```
Edit the HTML file:
```bash
    sudo nano /var/www/alternatives/alternate-index.html
```

### Step 5 - Configure Nginx
We now need to create a configuration file for the alternative page
```bash
    sudo nano /etc/nginx/sites-enabled/alternatives  
```
Sample configuration:
```bash
    server {
        listen 8081;
        listen [::]:8081;
        root /var/www/alternatives;
        index alternate-index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }  
```
> [!Note]
>  try_files $uri $uri/ =404; if a page name is not found within the given structure a 404 page not found page will be displayed instead

### Step 6 - Restart Nginx
After completing any changes to the config or HTML files, you must restart the Nginx server for the changes to take effect
```bash
    sudo service nginx restart
```
> [!Note]
>  after restarting you can check the status of the service using the command from step 3:
> ```bash
>       systemctl status nginx.service
> ```

### Step 7 - Test the changes
You can now see the new alternative HTML start page by entering the IP address with the port defined inthe config file:
```bash
    http://ip-address:8081/
```
## Setup Git

### Step 1 - Install Git
Install Git using the following command:
```bash
sudo apt install git -y
```

### Step 2 - Setup User setup
Setup your username using the following command:
```bash
git config --global user.name "User Name"
```
And you email:
```bash
git config --global user.email "user@email.com"
```
> [!Note]
> -y (yes) confirms the installation

### Step 2 - Generate SSH key for Git
Generate an SSH key as previously described in [**Setup and copy SSH keys - Step 1**](#Step-1-Generate-an-SSH-key-pair)

### Step 3 - Copy your public key

Open your public key using the cat editor:
```bash
cat /c/Users/UserName/.ssh/your-key-name.pub
```
and copy the key information.

### Step 4 - Add your key to Git

Go to your github profile and in the settings, clicks on SSH and GPG keys. Select New SSH key. Give it a title, such as "Virtual Server github key" and paste the key into the key field. Click on Add SSH key to complete the setup.


## Conclusion

### Congratulations, you have now completed the introduction to setting up a Virtual Server.
#### By reaching the end of this walkthrough, you should now be able to achieve the following:
    - Create and use SSH keys for secure authentication
    - Disable password authentication for a more secure login process using SSH keys
    - Install, configure, and manage an Nginx web server
    - Using custom web pages on your server
    - Setting up your username and email as well as an SSH key for Git and Git hub
#### Your server is now ready to be used for whatever you can imagine, be it hosting websites or deploying applications or just tinkering around to see what else you can achieve.
