# Linux Services

As per details shared by the development team, the new application release has some dependencies on the back end. There are some packages/services that need to be installed on all app servers under `Stratos Datacenter`. As per requirements please perform the following steps:

a. Install `squid` package on all the application servers.

b. Once installed, make sure it is enabled to start during boo



My Solution:

<pre><code><strong># SSH into all 3 app servers and run the following command
</strong><strong>
</strong><strong>sudo dnf install squid -y; sudo systemctl enable --now squid;
</strong></code></pre>
