# Apach file changes

<!-- crm/public 
TO DO: Get httpd-vhost.conf from fresh machine.
-->

## httpd-vhost.conf file

The httpd-vhost.conf file (in this folder) points at:

/usr/local/var/www/crm/public

And the DocumentRoot at the top pointed at: /usr/local/var/www

The Fix:
/usr/local/var/www/crm/public does not exist, so changed in 3 places to:

/Users/helix/Library/Data/profile/crm/account/public



Both of these are working with the above:
http://localhost:8080/
http://localhost:8080/crm/public/install.php



## httpd.conf file

Show all Apache listening ports with process info

    sudo lsof -i -P -n | grep LISTEN | grep apache



On Mac, in httpd.conf  add permission for Apache for default root and/or custom root

DocumentRoot "/usr/local/var/www"
<Directory "/usr/local/var/www">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>


install.php had to be included:

http://localhost:8080/crm/public/install.php

Do we need to add to httpd.conf ...
http://192.168.1.202:8080/

Not sure if this is the case:
Works after changeing <?php to <?phpX then back, and deleted index.html file in this folder:

http://localhost:8080/crm/public/install.php

This returns Forbidden. (note there is no account folder):

DocumentRoot "/usr/local/var/www/crm/public"
<Directory "/usr/local/var/www/crm/public">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

Didn't work

DocumentRoot "/Users/helix/Library/Data/profile/crm/account/public"
    <Directory "/Users/helix/Library/Data/profile/crm/account/public">
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>




Didn't resolve.
Do we need to change path in httpd-vhost.conf

sudo chmod 755 /usr/local/var/www/crm/public
sudo chmod +x /usr/local/var/www/crm/public


<Directory "/Users/helix/Library/data/profile/crm/public/">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>


Important: Add the second part back:

<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>

#
# The following lines prevent .htaccess and .htpasswd files from being 
# viewed by Web clients. 
#
<Files ".ht*">
    Require all denied
</Files>








sudo killall httpd

Both of these work when pointed at brew services start httpd )


    /usr/local/bin/httpd -D FOREGROUND



    brew services start httpd


Watch errors:

    tail -f /usr/local/var/log/httpd/error_log

Reveals:

client denied by server configuration: /usr/local/var/www/

FSuite.sh updates your Apache config file. You can see it's location here:

    brew --prefix apache2

Older Intel Mac returns: /usr/local/opt/httpd
And suite.sh updates the config files reside at:
/usr/local/etc/httpd/httpd.conf
/usr/local/etc/httpd/extra/httpd-vhosts.conf

New Macs get updated at:
/opt/homebrew/etc/httpd/httpd.conf


# Got to the httpd and edit the httpd.conf file

    sudo nano httpd.conf

# Or
    brew install micro
    sudo micro 

    ctrl-S is save
    ctrl-O is save in nano

# Just restarting wasn't enough
brew services start httpd

# Start Apache manually to test
/usr/local/bin/httpd -D FOREGROUND

sudo brew services start httpd


403 Forbidden

Change helix to your name in the following:

# Make sure the directory exists and has proper permissions
mkdir -p /Users/helix/Library/data/profile/crm/account/public
chmod 755 /Users/helix/Library/data/profile/crm/account/public

chmod 755 /usr/local/var/www/crm/public

chmod 755 /usr/local/var/www

Returns: event not found
# Create a test index file if it doesn't exist
echo "<h1>Hello from newsite!</h1>" > /Users/helix/Library/data/profile/crm/account/public/index.html


