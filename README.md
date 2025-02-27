# Apache Web Server Deployment  
**By: Hala Assi**  

This report documents the steps I took to deploy an Apache web server, configure virtual hosts, and troubleshoot network and DNS issues. Below is a detailed breakdown of the commands used, configurations applied, and their purposes.

---

## **1. List of All Commands Used with Descriptions**  

### **1.1 Setting Up Apache and Checking Its Status**  
| **Command** | **Description** |  
|-------------|-----------------|  
| `sudo apt update` | Updates the package lists to ensure the latest versions are available. |  
| `sudo apt install apache2 -y` | Installs the Apache web server. |  
| `sudo systemctl enable apache2` | Ensures Apache starts automatically after a system reboot. |  
| `sudo systemctl start apache2` | Starts the Apache web server. |  
| `sudo systemctl restart apache2` | Restarts Apache after making configuration changes. |  
| `sudo systemctl status apache2` | Checks the current status of Apache to confirm it’s running. |  

---

### **1.2 Checking Network & Connectivity**  
| **Command** | **Description** |  
|-------------|-----------------|  
| `ifconfig` or `ip a` | Displays network interface information, including the VM’s IP address. |  
| `ping 192.168.1.112` | Tests if the VM’s IP address is reachable from my Mac. |  

---

### **1.3 Configuring Virtual Hosts (Subdomains)**  
| **Command** | **Description** |  
|-------------|-----------------|  
| `sudo nano /etc/apache2/sites-available/home.halaassi.com.conf` | Opens the virtual host configuration file for the `home` subdomain. |  
| `sudo nano /etc/apache2/sites-available/about.halaassi.com.conf` | Opens the virtual host configuration file for the `about` subdomain. |  
| `sudo nano /etc/apache2/sites-available/contact.halaassi.com.conf` | Opens the virtual host configuration file for the `contact` subdomain. |  
| `sudo a2ensite home.halaassi.com.conf` | Enables the `home` subdomain in Apache. |  
| `sudo a2ensite about.halaassi.com.conf` | Enables the `about` subdomain in Apache. |  
| `sudo a2ensite contact.halaassi.com.conf` | Enables the `contact` subdomain in Apache. |  
| `sudo a2dissite 000-default.conf` | Disables the default Apache site to avoid conflicts. |  
| `sudo apachectl -S` | Displays the list of active virtual hosts and their configurations. |  

---

### **1.4 Managing Website Files & Permissions**  
| **Command** | **Description** |  
|-------------|-----------------|  
| `sudo mkdir -p /var/www/html/home` | Creates the directory for the `home` subdomain. |  
| `sudo mkdir -p /var/www/html/about` | Creates the directory for the `about` subdomain. |  
| `sudo mkdir -p /var/www/html/contact` | Creates the directory for the `contact` subdomain. |  
| `sudo chown -R www-data:www-data /var/www/html/` | Changes ownership of all web files to Apache’s user (`www-data`). |  
| `sudo chmod -R 755 /var/www/html/` | Sets the correct file permissions for Apache to read and serve files. |  

---

### **1.5 Fixing Network**  
| **Command** | **Description** |  
|-------------|-----------------|  
| `sudo nano /etc/hosts` | Opens the hosts file to map subdomains to the VM’s IP address. |  
 
```
192.168.1.112 halaassi.com
192.168.1.112 home.halaassi.com
192.168.1.112 about.halaassi.com
192.168.1.112 contact.halaassi.com
```
---

## **2. Apache Configurations with Descriptions**  

### **2.1 Virtual Host Configuration for `home.halaassi.com`**  
**File:** `/etc/apache2/sites-available/home.halaassi.com.conf`  
**Purpose:** Configures Apache to serve requests for the `home.halaassi.com` subdomain.  
```apache
<VirtualHost *:80>
    ServerName home.halaassi.com
    DocumentRoot /var/www/html/home

    <Directory /var/www/html/home>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/home_error.log
    CustomLog ${APACHE_LOG_DIR}/home_access.log combined
</VirtualHost>
```
### **2.2 Virtual Host Configuration for `about.halaassi.com`**  
**File:** `/etc/apache2/sites-available/about.halaassi.com.conf`  
**Purpose:** Configures Apache to serve requests for the `about.halaassi.com` subdomain.  
```apache
<VirtualHost *:80>
    ServerName about.halaassi.com
    DocumentRoot /var/www/html/about

    <Directory /var/www/html/about>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/about_error.log
    CustomLog ${APACHE_LOG_DIR}/about_access.log combined
</VirtualHost>
```
### **2.3 Virtual Host Configuration for `contact.halaassi.com`**  
**File:** `/etc/apache2/sites-available/contact.halaassi.com.conf`  
**Purpose:** Configures Apache to serve requests for the `contact.halaassi.com` subdomain.  
```apache
<VirtualHost *:80>
    ServerName contact.halaassi.com
    DocumentRoot /var/www/html/contact

    <Directory /var/www/html/contact>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/contact_error.log
    CustomLog ${APACHE_LOG_DIR}/contact_access.log combined
</VirtualHost>
```

### **2.3 Updating Apache’s Main Configuration ( `apache2.conf`**  
**File:** `/etc/apache2/apache2.conf`  
**Purpose:** Ensures Apache can handle multiple virtual hosts correctly..  
```apache
NameVirtualHost *:80
```



