<p><img src="https://s24255.pcdn.co/wp-content/uploads/2015/04/OwnCloud.png" alt="Alt text" title=""></p>

<h1>ownCloud Quick Start Guide</h1>

<p>Installing, Configuring, and Connecting to the ownCloud Server</p>

<h3>Abstract</h3>

<p>The ownCloud Quick Start Guide is designed to help you quickly install, configure, and connect to an ownCloud server. This guide is aimed primarily at administrators and users.</p>

<h2>Chapter 1. Should I Use This Guide?</h2>

<p>The Quick Start Guide enables you to install, configure, and use your ownCloud server based on a set of assumptions about you.</p>

<p><strong>You should use this guide if the following assumptions apply to your environment:</strong></p>

<ul>
<li><p>You are an administrator installing and configuring an ownCloud server on a local virtual environment OR you already have a virtual environment that can host the ownCloud server (a DHCP Server is recommended).</p></li>
<li><p>You are an administrator enabling users to connect to the ownCloud server using the server’s IP address and port 8080.</p></li>
<li><p>You are an administrator adding user accounts.</p></li>
<li><p>You are a user connecting to the ownCloud server via a desktop or mobile client.</p></li>
</ul>

<p>Some of the tasks in this guide use example information. You should use values that are specific to your environment.</p>

<p>If your environment does not fit with the first assumption, you can use the following resource to assist you with the installation of a <a href="https://www.virtualbox.org/wiki/Downloads">VirtualBox</a> (a local virtual application):</p>

<h2>Chapter 2. Installation and Configuration</h2>

<h3>2.1 System Requirements</h3>

<p><strong>ownCloud includes the ownCloud server, which runs on the following Linux distributions:</strong></p>

<ul>
<li>CentOS Linux 6 and 7</li>
<li>Debian 7 and 8</li>
<li>Fedora 27 and 28</li>
<li>Red Hat Enterprise Linux 6 and 7</li>
<li>SUSE Linux Enterprise Server 12 with SP1, SP2, and SP3</li>
<li>openSUSE Tumbleweed and Leap 15.0, 42.3</li>
<li>Ubuntu 16.04 and 18.04</li>
</ul>

<p><strong>TIP</strong> For a detailed list of recommended and supported options, see <a href="https://doc.owncloud.org/server/10.0/admin_manual/installation/system_requirements.html">System Requirements</a>.</p>

<h3>2.2 Installing ownCloud Server on Linux</h3>

<p>There are 3 methods for deploying the ownCloud server. Select the best method that applies to your environment and click the appropriate link for installation procedures:</p>

<ol>
<li><strong>Manual Installation on Linux</strong> (Preferred Method) - For installation procedures, see <a href="https://doc.owncloud.org/server/10.0/admin_manual/installation/source_installation.html">Manual Installation on Linux</a>.</li>
<li><strong>ownCloud X Server Appliance</strong> (Recommended for testing) - For installation procedures, see <a href="https://oc.owncloud.com/rs/038-KRL-592/images/Whitepaper_User_Guide_Applicance_ENG.pdf">Setting up Your ownCloud</a>. For additional information, see <a href="https://doc.owncloud.com/server/latest/admin_manual/appliance/">ownCloud X Appliance Documentation</a>.</li>
<li><strong>Linux Package Manager</strong> - For installation procedures, see <a href="https://doc.owncloud.org/server/10.0/admin_manual/installation/linux_installation.htm">Linux Package Manager Installation</a>.</li>
</ol>

<p><strong>WARNING</strong> This package is not recommended for production environments.</p>

<h4>2.2.1 Installing and Configuring ownCloud on Centos 7 Linux Distribution</h4>

<p>The following procedure explains how to manually install ownCloud server on Centos 7.</p>

<p><strong>To install and configure ownCloud on Centos 7 Linux distribution</strong></p>

<ol>
<li><p>Install the PHP prerequisite modules on the console.</p>

<pre><code>  yum install php72-php-pecl-zip php-xml *xmlwriter* php72-php-intl -y
</code></pre></li>
<li><p>Download the ownCloud-10.x.x.zip source file.</p>

<pre><code>  wget https://download.owncloud.org/community/owncloud-10.x.x.zip
</code></pre></li>
<li><p>Extract the ownloud-10.x.x.zip source file.</p>

<pre><code>  unzip owncloud-10.0.10.zip
</code></pre></li>
<li><p>Move the extracted directory into the Apache document directory</p>

<pre><code>  mv owncloud /var/www/
</code></pre></li>
<li><p>Copy the ownCloud Apache configuration file from the <strong>Manual Installation on Linux</strong> page, and then <strong>Configure Apache Web Server</strong> section.</p>

<pre><code>  vi /etc/httpd/conf.d/owncloud.conf:


  Alias /owncloud "/var/www/owncloud/"


  &lt;Directory /var/www/owncloud/&gt;
  Options +FollowSymlinks
  AllowOverride All


  &lt;IfModule mod_dav.c&gt;
  Dav off
  &lt;/IfModule&gt;


  SetEnv HOME /var/www/owncloud
  SetEnv HTTP_HOME /var/www/owncloud


  &lt;/Directory&gt;
</code></pre></li>
<li><p>If SELinux is enabled, set the correct context on the ownCloud Apache directory.</p>

<pre><code>  chcon --verbose --recursive --reference /var/www/html /var/www/owncloud
</code></pre></li>
<li><p>Change ownership of Apache ownCloud directory to the Apache user.</p>

<pre><code>  chown -R apache:apache /var/www/owncloud/
</code></pre></li>
<li><p>Open the firewall port for needed for ownCloud Http service.</p>

<pre><code>  firewall-cmd --permanent --add-port=80/tcp &amp;&amp; firewall-cmd –reload
</code></pre></li>
<li><p>Start and enable the Apache service.</p>

<pre><code>     systemctl start httpd &amp;&amp; systemctl enable httpd
</code></pre></li>
</ol>

<p>In the web browser of your choice (e.g., Google Chrome, Internet Explorer, Firefox, etc.), enter the IP address of the ownCloud server and login with the default administrator credentials.</p>

<p><strong>TIP</strong> The default administrator credentials are <em>Administrator</em> and the root password.</p>

<h2>Chapter 3. Connecting Users to the ownCloud Server</h2>

<h3>3.1 Enabling Users to Connect to the ownCloud Server</h3>

<p>The following procedure explains how to enable a user to connect to the ownCloud server using the server's IP address and port 8080.</p>

<p><strong>To connect users to the ownCloud server using the CLI</strong></p>

<ol>
<li><p>Configure the Apache service to listen on port 8080.</p>

<pre><code>     vi /etc/httpd/conf/httpd.conf and add the following line: Listen &lt;ipaddress&gt;:8080
</code></pre></li>
<li><p>Open the firewall port 8080.</p>

<pre><code>     firewall-cmd --permanent --add-port=8080/tcp &amp;&amp; firewall-cmd –reload
</code></pre></li>
<li><p>Restart the Apache service.</p>

<pre><code>     systemctl restart httpd
</code></pre></li>
<li><p>In the web browser of your choice (e.g., Google Chrome, Internet Explorer, Firefox, etc.), enter the IP address of the ownCloud server and login with the default administrator credentials.</p></li>
</ol>

<p><strong>TIP</strong> The default administrator credentials are <em>Administrator</em> and the root password.</p>

<h2>Chapter 4. Adding a User Account to the ownCloud Server</h2>

<h3>4.1 Adding a User Account</h3>

<p>The following procedure explains how to add a user account to the ownCloud server.</p>

<p><strong>To add a user account using the web UI</strong></p>

<ol>
<li>Click the <strong>Administrator</strong> drop-down menu.</li>
<li>Select <strong>Users</strong>.</li>
<li>In the <strong>Username</strong> field, enter a username.</li>
<li>In the <strong>E-Mail</strong> field, enter a valid e-mail address.</li>
<li>Click <strong>Create</strong>.</li>
</ol>

<h2>Chapter 5. Connecting to the ownCloud Server Using External Devices</h2>

<p>The ownCloud desktop client enables you to keep your data synced and gives you access to the latest files wherever you are.</p>

<p><strong>To connect to one of the following desktop clients, click the appropriate link and follow the download procedure:</strong></p>

<ul>
<li><p><a href="https://owncloud.org/download/#owncloud-desktop-client-macos">ownCloud Desktop Client for MacOS</a></p></li>
<li><p><a href="https://owncloud.org/download/#owncloud-desktop-client-windows">ownCloud Desktop Client for Windows</a></p></li>
<li><p><a href="https://owncloud.org/download/#owncloud-desktop-client-linux">ownCloud Desktop Client for Linux</a></p></li>
</ul>

<h3>5.2 Connecting to the ownCloud Server Using a Mobile Device</h3>

<p>These mobile apps enable you to access, sync, and upload your data on the run. Mobile apps are available in both the <a href="https://itunes.apple.com/us/app/owncloud/id543672169?ls=1&amp;mt=8">Apple Store</a> and the <a href="https://play.google.com/store/apps/details?id=com.owncloud.android">Google Play Store</a>.</p>

<p><strong>To connect to one of the following mobile apps, click the appropriate link and follow the download procedure:</strong></p>

<p><a href="https://owncloud.org/download/#owncloud-mobile-apps-ios">iOS</a></p>

<p><a href="https://owncloud.org/download/#owncloud-mobile-apps-android">Android</a></p>
