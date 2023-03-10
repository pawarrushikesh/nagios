# nagios
Nagios server installation step by step on Cent OS 7
# Step 1: Connect to your server
Connect to your server via SSH as user root, use the following command:
ssh root@IP_ADDRESS
Once logged in, make sure that your server is up-to-date by running the following command:

yum update

# Step 2: Install LAMP
yum install httpd mariadb-server php php-mysql

systemctl start httpd.service
systemctl start mariadb.service
systemctl enable httpd.service
systemctl enable mariadb.service

mysql_secure_installation

# Step 3: Install the required packages
yum install gcc glibc glibc-common wget gd gd-devel perl postfix

# Step 4: Download and Install Nagios Core
cd /tmp
wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.2.tar.gz
tar xzf nagioscore.tar.gz
cd /tmp/nagioscore-nagios-4.4.2
./configure
make all
make install-groups-users
usermod -a -G nagios apache
make install
make install-daemoninit
make install-config
make install-commandmode
make install-webconf
systemctl restart httpd

# Step 5: Create nagiosadmin User Account
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin

# Step 6: Install Nagios Plugins
yum install gcc glibc glibc-common make gettext automake autoconf wget openssl-devel net-snmp net-snmp-utils epel-release perl-Net-SNMP

cd /tmp
wget --no-check-certificate -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/archive/release-2.2.1.tar.gz
tar zxf nagios-plugins.tar.gz

cd /tmp/nagios-plugins-release-2.2.1/
./tools/setup
./configure
make
make install

# Step 7: Accessing Nagios Core
systemctl start nagios

if you are using OS firewall then used below command or else hardware using then allow 80 port to open in firelwall
First add your port 80 rule with the following linux command:
firewall-cmd --zone=public --add-port=80/tcp --permanent
Once you add the above firewall rule, reload the firewall service with this command:
firewall-cmd --reload

To access Nagios Core, open your browser and navigate to http://YOUR-IP-ADDRESS/nagios and log in using the nagiosadmin user account which you have created in one of the previous steps in this tutorial.

After you have successfully logged in, you will be presented with the Nagios Core home screen as shown in the image below:

