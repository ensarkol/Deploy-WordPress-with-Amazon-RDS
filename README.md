# Creating a MySQL Database with Amazon RDS

I created an Amazon RDS database and configured it to allow access to specific entities.

#Creating an EC2 Instance


I created an Amazon EC2 instance to run our WordPress site. Amazon EC2 provides highly configurable server instances on demand. On an EC2 instance, we can run a WordPress site that can be accessed by users from anywhere.

# Configuring Your Amazon RDS Database

At this point, we have created an Amazon RDS database and an EC2 instance. At this point, I have configured the Amazon RDS database to allow access to specific entities.

# Configuring WordPress on EC2

Up to this point, I've done a lot of configuration setup, created an Amazon RDS instance and an EC2 instance, allowed network access from your EC2 instance to your Amazon RDS instance, learned how to use SSH to connect to your EC2 instance, and configured a database user to be used by WordPress. Finally, we will finish making our WordPress site live. To complete the steps in this module, we will need to use SSH to connect to our EC2 instance.

To run WordPress, we need to run a web server in our EC2 instance. 
```
sudo yum install -y httpd
```
I ran the following command in my terminal to start the Apache web server:

####
```
sudo systemctl start apache2
```
By visiting the public DNS of our EC2 instance in our browser we will see that our Apache web server is running and the security groups are configured correctly.

# Download and configure Wordpress

In this step, we will download the WordPress software and set up the configuration. First, I downloaded and unzipped the software by running the following commands in our terminal:


wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz


I ran ls to view the contents of your directory, I saw a tar file and a directory named wordpress with uncompressed content.


$ ls


The output should look like the following:

[ec2-user@~]$ ls
latest.tar.gz wordpress

I changed the directory to the wordpress directory and created a copy of the default configuration file using the following commands:

cd wordpress
cp wp-config-sample.php wp-config.php

Then, I opened the wp-config.php file using the nano editor by running the following command.

nano wp-config.php

We need to edit two configuration fields.
First, I edited the database configuration by modifying the following lines:

// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'database_name_here' );

/** MySQL database username */
define( 'DB_USER', 'username_here' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password_here' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

The values should be:
DB_NAME: “wordpress”
DB_USER: The name of the user you created in the database
DB_PASSWORD: TPassword for the user we created
DB_HOST: Host name of the database


The second configuration section we need to configure is Authentication Unique Keys and Salts. It looks like the following in the configuration file:



/**#@+
 * Authentication Unique Keys and Salts.
 *
 * Change these to different unique phrases!
 * You can generate these using the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}
 * You can change these at any point in time to invalidate all existing cookies. This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );


You should find a link that targets an API endpoint used to generate a randomly generated security key sequence for WordPress, you can find this link in the resources section.You can replace the entire content in that section with the content from the link.You can save and exit from nano by entering CTRL+O [ENTER] followed by CTRL+X.

# Deploying Wordpress

In this step, we will make sure that our Apache web server handles incoming requests for WordPress.First, let's install the application dependencies we need for WordPress. Let's run the following command in our terminal.

sudo apt update
sudo apt install -y mariadb-server php8.2

Second, let's move to the appropriate directory by running the following command:

cd /home/ec2-user

Next, let's copy our WordPress application files to the /var/www/html directory used by Apache.

sudo cp -r wordpress/* /var/www/html/

Finally, let's restart the Apache web server to get the changes.

sudo systemctl restart apache2

That's it. We have a live, publicly accessible WordPress installation using a fully managed MySQL database on Amazon RDS.



