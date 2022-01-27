!#bin/bash
adduser --disabled-password --shell /bin/bash --gecos "User" ftpadmin
echo "ftpadmin:$1" | chpasswd
sudo usermod -aG sudo ftpadmin
sed -i '57,58 s/^/#/' /etc/ssh/sshd_config
echo "PermitRootLogin no">>/etc/ssh/sshd_config
echo "PubkeyAuthentication no">>/etc/ssh/sshd_config
echo "PasswordAuthentication yes">>/etc/ssh/sshd_config
echo "AllowUsers ftpadmin ubuntu">>/etc/ssh/sshd_config
echo "AllowGroups ftpadmin sudo">>/etc/ssh/sshd_config
sudo systemctl restart ssh.service
sudo systemctl restart sshd

sudo apt-get install vsftpd -y     
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
sudo cp /etc/vsftpd.conf  /etc/vsftpd.conf_default
sudo useradd -m testuser
echo "testuser:$2" | chpasswd
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
sudo systemctl restart vsftpd

su ftpadmin
bash
