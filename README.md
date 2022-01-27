### Add_UserAccount_and_FTPUser.sh is the Bash script which creates a user account to ssh into server and a ftp user to manage artifacts

#### To run the bash script , Go to the directory where shell script is located and run 
####  ```./Add_UserAccount_and_FTPUser.sh password1 password2```
 FirstArgument: Password for UserAccount to SSH , SecondArgument: Password for FTPUser
#### adduser --disabled-password --shell /bin/bash --gecos "User" SSHUser
 This command creates user with username SSHUser
#### echo "SSHUser:$1" | chpasswd
 To set password for SSHUser , Pass the password in firstargument of shell script
#### sudo usermod -aG sudo SSHUser
 To Give sudo priveledges to SSHUser , we need to add it in sudo group using this command 
#### sed -i '57,58 s/^/#/' /etc/ssh/sshd_config
 To comment line no. 57 and 58 in sshd_config file because to append PasswordAuthentication yes in sshd_config file we need to first comment PasswordAuthentication no which is by default in sshd_config file.
#### echo "PermitRootLogin no">>/etc/ssh/sshd_config
 This command will append ```PermitRootLogin no``` in sshd_config file
#### echo "PubkeyAuthentication no">>/etc/ssh/sshd_config
 This command will append ```PubkeyAuthentication no``` in sshd_config file
#### echo "PasswordAuthentication yes">>/etc/ssh/sshd_config
 This command will append ```PasswordAuthentication yes``` in sshd_config file
#### echo "AllowUsers SSHUser ubuntu">>/etc/ssh/sshd_config
 This command will append ```AllowUsers SSHUser ubuntu``` in sshd_config file and only SSHUser and ubuntu will be allowed to ssh into server.
#### echo "AllowGroups SSHUser sudo">>/etc/ssh/sshd_config 
 This command will append ```AllowGroups SSHUser sudo``` in sshd_config file and only SSHUser and sudo group will be allowed.
#### sudo systemctl restart ssh.service
 To restart ssh
#### sudo systemctl restart sshd
 
#### sudo apt-get install vsftpd -y    
 To install ftp server
#### sudo systemctl start vsftpd
 To start ftp server
#### sudo systemctl enable vsftpd
 To enable ftp server so that on every boot it starts
#### sudo cp /etc/vsftpd.conf  /etc/vsftpd.conf_default
 To copy and create default vsftpd.conf file
#### sudo useradd -m ftpuser
 To create a FTPUser
#### echo "ftpuser:$2" | chpasswd
 The will set the second argument as password for ftpuser 
#### sudo ufw allow 20/tcp
 To allow port 20 for ftp 
#### sudo ufw allow 21/tcp
 To allow port 21 for ftp 
#### sudo systemctl restart vsftpd
 To restart ftp server
 
#### su SShUser
 To change user to SSHUser
#### bash
