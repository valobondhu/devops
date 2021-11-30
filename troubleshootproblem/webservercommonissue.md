# Problem One :  ssl issue in linux 
Problem :: fatal: unable to access '': SSL certificate problem: certificate has expired. 
### solve : In suse machine
	rm /usr/share/pki/trust/DST_Root_CA_X3.pem
	sudo update-ca-certificates

# Problem Two: domain _ problem
	Bad Request
	Your browser sent a request that this server could not understand.
	Additionally, a 400 Bad Request error was encountered while trying to use an ErrorDocument to handle the request.
### solve : 
	This is due to the update RFC 3986, which claims that underscores are unsafe in virtual host servernames and other elements.
	So remove _ from your domain name
	...
	remove the underscore (_) from the ServerName directive, as well as the hostname in /etc/hosts.


