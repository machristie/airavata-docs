## Troubleshooting FAQ
<br><b class="blue"> Q1.</b> I have setup my own gateway and Airavata. When I log into the gateway I cannot create Compute resources. What should I do?</br>
<b class="blue">Answer: </b> In your pga_config.php (in folder .../testdrive/app/config) under heading 'Portal Related Configurations' set 'super-admin-portal' => false, to true.</br>
<br><b class="blue">Q2.</b> I don't get notifications when users create new accounts in my gateway. Why?</br>
<b class="blue">Answer: </b> That's because you have not defined an email address in <br>'admin-emails' => ['xxx@xxx.com','yyy@yyy.com']. Here you can add one or many.</br>
<br><br><b class="blue"> Q3.</b>  I am not receiving email notifications from compute resoures for job status changes. What should I do?</br>
<b class="blue">Answer: </b> In airavata-server.properties please locate and set your email account information.
<pre><code>email.based.monitor.host=imap.gmail.com
email.based.monitor.address=airavata-user@kuytje.nl
email.based.monitor.password=zzzz
email.based.monitor.folder.name=INBOX
email.based.monitor.store.protocol=imaps (either imaps or pop3)</pre></code>
<br><br><b class="blue"> Q4.</b>  In my Airavata log I have error messages like
<br>ERROR org.apache.airavata.api.server.handler.AiravataServerHandler  - Error occurred while retrieving SSH public keys for gateway
<br>ERROR org.apache.airavata.credential.store.server.CredentialStoreServerHandler  - Error occurred while retrieving credentials
<br></br>What should I do?<br>
<br><b class="blue">Answer: </b> This could be due to missing tables in your credential store database. Check whether CREDENTIALS and COMMUNITY_USER tables exits. If not create then using
<pre><code>CREATE TABLE COMMUNITY_USER
(
        GATEWAY_ID VARCHAR(256) NOT NULL,
        COMMUNITY_USER_NAME VARCHAR(256) NOT NULL,
        TOKEN_ID VARCHAR(256) NOT NULL,
        COMMUNITY_USER_EMAIL VARCHAR(256) NOT NULL,
        PRIMARY KEY (GATEWAY_ID, COMMUNITY_USER_NAME, TOKEN_ID)
);
</pre></code>
<pre><code>CREATE TABLE CREDENTIALS
(
        GATEWAY_ID VARCHAR(256) NOT NULL,
        TOKEN_ID VARCHAR(256) NOT NULL,
        CREDENTIAL BLOB NOT NULL,
        PORTAL_USER_ID VARCHAR(256) NOT NULL,
        TIME_PERSISTED TIMESTAMP DEFAULT NOW() ON UPDATE NOW(),
        PRIMARY KEY (GATEWAY_ID, TOKEN_ID)
);
</pre></code>
<br><br><b class="blue"> Q5.</b>  I cannot login to my Compute Resource and launch jobs from Airavata using the SSH key I generated. What should I do?
<br><b class="blue">Answer: </b> Steps to use generated SSH key<br>
        - Generate SSH key + token using Credential Store<br>
        - Add the generated token to your resource through PGA --> Admin Dashboard --> Gateway Preferences<br>
        - Add the generated SSH key<br>

<br><b class="blue"> Q6.</b> When installing PGA in MAC i got below error after updating the composer.
    	<br>- Error<br>
    	Mcrypt PHP extension required.<br>
    	Script php artisan clear-compiled handling the post-update-cmd event returned with an error<br>
      		[RuntimeException]<br>
      		Error Output:
<br><b class="blue">Answer: </b>
      	Install mcrypt installation;
    	<a href="http://coolestguidesontheplanet.com/install-mcrypt-php-mac-osx-10-10-yosemite-development-server/" target="_blanks">mcrypt Installation Steps</a>

<br><b class="blue"> Q7.</b> After following the required steps only the home page is working and some images are not shown properly.
    <br><b class="blue">Answer: </b>If you are facing this behavior first check whether you have enabled mod_rewrite module in apache webserver.
    <br>And also check whether you have set AllowOverride All in the Vhost configuration file in apache web server. <br>(e.g file location is /etc/apache2/sites-available/default and there should be two places where you want to change)
<br><pre><code>
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot /var/www/html/portal/public
    ServerName pga.example.com
    <Directory "/var/www/html/portal/public">
       AllowOverride all
    </Directory>
    ErrorLog logs/pga_error_log
    CustomLog logs/pga--access_log common
</VirtualHost></code></pre>
    <br>
<br><b class="blue"> Q8.</b> I get the Error message Permission Denied to app/storage directory.<br>
    <br><b class="blue">Answer: </b>Execute the following command and grant all permissions; <pre><code> sudo chmod -R 777 app/storage</code></pre>

<br><b class="blue"> Q9.</b> In Ubuntu environment when executing sudo composer update it fails with message "Mcrypt PHP extension required".
 <br><b class="blue">Answer: </b>To fix this install PHP mcrypt extension by following the below steps;
 <pre><code>sudo apt-get install php5-mcrypt</code></pre>
  Locate mcrypt.so ,to get its location
  Locate mcrypt.ini and open the mcrypt.ini file
    <pre><code>sudo pico /etc/php5/mods-available/mcrypt.ini</code></pre>
  Change the at line a extension=<location of the mcrypt.so file> eg:/usr/lib/php5/20121212/mcrypt.so Save changes.
  Execute the command:  <pre><code>sudo php5enmod mcrypt</code></pre>
  Now restart the apache server again and test PGA web-interface

<br><b class="blue"> Q10.</b> When tried to login or create a new user account an Error is thrown which is similar to PHP Fatal error:  SOAP-ERROR: Parsing WSDL: Couldn't load from...
    <br><b class="blue">Answer: </b> If you face this kind of an error first check whether you have enabled PHP SOAP and OpenSSL extensions. If even after enabling them the issue is still occurring try updating the PHP OpenSSL extension. (Using command like yum update openssl)
    
<br><b class="blue"> Q11.</b> If you are seeing an error similar to following in your airavata.log <br>
Error: ERROR org.apache.airavata.registry.core.app.catalog.impl.StorageResourceImpl  - Error while retrieving storage resource...
       javax.persistence.NoResultException: Query "SELECT p FROM StorageResource p WHERE p.storageResourceId =:param0" selected no result, but expected unique result.
<br><b class="blue">Answer: </b> Add storage resource ID in to the pga_config.php in your gateway code/app/config directory.  
         
<br><b class="blue"> Q12.</b> I am getting error <br>
2016-05-19 16:17:08,225 [main] ERROR org.apache.airavata.server.ServerMain  - Server Start Error:
java.lang.RuntimeException: Failed to create database connection pool. <br>

What should i do?
<br><b class="blue">Answer: </b> Airavata cannot create database connection ebcause the mysql jar is not eixsting. Please follow step 8 of documentation in Installation --> Airavata --> Airavata Installation


       