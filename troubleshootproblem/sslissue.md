# Problem One :  ssl issue in linux 
Problem :: fatal: unable to access '': SSL certificate problem: certificate has expired. 
solve : In suse machine 
		rm /usr/share/pki/trust/DST_Root_CA_X3.pem
		sudo update-ca-certificates
