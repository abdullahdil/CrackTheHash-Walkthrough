# CrackTheHash-Walkthrough
Password cracking is one of the important and necessary steps. As you have already come across organizations don’t store passwords in plain-text format rather they use hashing algorithms to secure their passwords.  This room will help you to know about different types of hashed passwords and tools to crack them :>

Detailed Walkthrough FIle Inclusion Vulnerability by {ParadoxThief}


File Inclusion is One of the Most Critical Vulnerability and is included in the OWASP top 10.

Main Target in this Attack : web server of the Target

Causes of the File Inclusion Attacks
	-Poor Server side Code (Input parameters not properly validated )
	-misconfigured system files

Results:
	-Attacker can include malicious files in ther server
	-Attacker can access sensitive(not permitted ) filees present in that server.
	
Basic Approach :
	Attacker can alter/manipulate the HTTP request parameters going to the server (basically change the requesting file names )
	
	
	Detailed Approach:
	
	When we enter a Url into the Adress bar of the search enginer.
	
	we type it basically like this 
	http://abc.com/?file=examplefile.php
		The Url simply means:
			http = we are using the Http protocol(requesting for a webpage)
			":" =   means that next here the previous thing ended
			// = means the authority is coming( here the authority is abc.com
			abc.com = domain 	
			.com is a top level domain
			abc is the domain label
			/ the directory path for the file (here it is / which is the default and is /var/www/html/)
			file=hacking.php = this is the actual querry that is telling the server that client wants this file.(the thing that we are going to exploit)
				
				
				
		The attacks manipulates the Filename that is contained in the string so that it can get unwanted files from the server.The attcker can try in multiple words on that filename 
		and the server will execute and return the file ( if it is present in the directory )
		
		
		
		
		The attacker not only can access the internal files but can also make the server install and execute the malicious files from the remote attackers surface.
		
		
		The File Inclusion attacks are of two types:
		
 					  1- Local File Inclusion (LFI):
 			In the Local File inclusion type We are going to exploit the Files already present on the webserver of the website being attacked.
			A poorly implmented server side ( inputs not sanitized and checked) can cause this type of vulnerability.
    2-Remote File Inclusion (RFI):
    		is a web vulnerability where an attacker tricks a web application into loading malicious code from a remote server, allowing for potential data theft or 
            complete system  compromise.
		
		
		
		Important preText Infomation that we need :
				
				The PHP Include() Function:
				
				In some words,what i have learned from various channels(I am not a php specialist , i dont even know how to write a php code ) is that when developers
				make website, they make this small components of the php files that can be reused later at various places.The developers can reuse the same code at multiple places
				and hence the need of chaging entirely everything vanishes(good thing on the developers side).
					
				When developers want to include one php file into the other php file they use the include fucntion( hence the name of the file suggests  that include A file into the 
				B file ).This inlcude fucntion simply fetches the code from the php A file and Write in the Php B file as if it was written in the B file already.
					
					    Lets understand this by an example
						file name = var.php
						
						suppose i write a php code.In this code I am sumply assighning the variable color = red and the variable car  = BMW.
						I save it ...Done.
						
						Now i make another php file 
						
						Here when i will run it, when the line 5 will execute,It will only outputs A at line 5 because the variable color adn car has not yet been assigned a value.
						At line 6, we used the include function, ‘var.php’
						when the code executed the var.php file now the compiler knows what are the values of the color and the car
						and now when the line 7 will execute, It wll print out the A red BMW.
						
						so we successfully included a php file in to another php file.....Simple as that.
						
						
				The PHP Require() Function:
						
						Oky , so the Php require Function is just like the include function but with a one great difference.
						• require Function:
                              • The require function is used to include and evaluate a specified PHP file during script execution.
                              • If the file specified in require cannot be found or included for any reason (such as file not existing or permission issues), 
		                         PHP will produce a fatal error and halt script execution.
                              • Syntax: require 'filename.php';
                                 

                        • include Function:
◇ The include function also includes and evaluates the specified PHP file during script execution.
◇ If the file specified in include cannot be found or included, PHP will produce a warning and continue script execution.
◇ Syntax: include 'filename.php';
								
								
					
				The PHP Require() Function:
				
						When require_once is used to include a file, PHP checks if the file has already been included elsewhere in the script execution.
						If the file has been included before (either by require_once, include_once, or require), PHP does not include it again.
						If the file hasn't been included yet, PHP includes it and marks it as included.


										
										
				
				Oky Sow that we have learned about the Important Functions of the Php that will help us in the FIle Inclusion, we can Proceed
				
				First of all, deploy the target machine and connect your Vm through the Open vpn with the tryhackme servers.
				After Deploying the machine, you will get a target IP, Run this on the firefox in your vpn.
				
						Path Traversal (dot-dot-slash) Attack:
							So basically the base of LFI attacks is the directory traversal vulnerability that allows the attacker to crawl to those places where the attacker shoudl not be.
							The main Contributor to this attack is the Linux Command “..” that means we want to crawl up in the heirarchy of the linux file system( oen directory).
							
							
							Some Important Files and the Information they contain: 


				What function causes path traversal vulnerabilities in PHP?
				
				Answer : file_get_contents


                 LFI Attacks Scenarios : 
                 	
				1. Suppose the web application provides two languages, and the user can select between the EN and AR
					Now See, When the Websites is using two languages, Suoppose English and the Ar(I dont know what that means).
					How does it work ? When we press on the EN, THe Website Actually send the “EN” int the URl to the server , so that it could load the English version of the Site
					
					http://webapp.thm/index.php?lang=EN.php
					Or
					http://webapp.thm/index.php?lang=AR.php
					In the backend, How this works is, when the server is executing the code for the Site, it counters a include Message
					
					This include function is telling the compiler that whatever is present in my brackets, include that to my code.In the function arguments we can see a $_Get["Lang"] thing.
					what this is actually doing is “getting the file whatever is included in it  ”.her we can see the Lang variable is included.and in our case if we want to see the English version
					of the website the value of the LANG will be “EN.php”.
					The include function includes the EN.php and our site runs in the English Version. 
					
					
				2. Next, In the following code, the developer decided to specify the directory inside the function.
					

					Now technically our get message will take us in the directory
						
						/var/www/http/languages
						
						to travel to the etc/passwd we can backtrack 4 times to be in the root directory and from there we can access /etc/passwd
						
						../../../../etc/passwd
						
						
						Give Lab #1 a try to read /etc/passwd. What would the request URI be?

						Answer : /lab1.php?file=/etc/passwd
				

						In Lab #2, what is the directory specified in the include function?
						
						Answer : includes
				


					If the developers has applied the check that only the Php file can be triggered then we can use the Null Byte %00 Injection.
					
						Give Lab #3 a try to read /etc/passwd. What is the request look like?
							
							/lab3.php?file=../../../../etc/passwd%00
						
						Which function is causing the directory traversal in Lab #4?
							
							file_get_contents
							
						
						Try out Lab #6 and check what is the directory that has to be in the input field?
						
							THM-profile
							
						
						Try out Lab #6 and read /etc/os-release. What is the VERSION_ID value?

							12.04
							
						
						
















							
						
						
						
				
					
					LF1 Can also be easily done through the LF1 Suite on the Kali linux.
						Remember whenever we access something from the servers, we are automatically in the /var/www/http/ directory by default.
						
						
