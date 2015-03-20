WebLogic on Docker
===============
Docker configurations to facilitate installation, configuration, and environment setup for developers. This project includes [dockerfiles](/dockerfiles/12.1.3/wls) for WebLogic 12c 12.1.3 with JDK 7.

## Based on Official Oracle Linux Docker images
For more information please read the [Docker Images from Oracle Linux](https://registry.hub.docker.com/_/oraclelinux/) page.

## How to build and run
This project offers Dockerfiles for WebLogic 12c (12.1.3) for the 'generic' distribution with JDK 7.
To download the generic WebLogic 12c package from the Oracle site, it's necessary to be authenticate in the site.
To do this, I use the method [Export cookies](http://www.pythian.com/blog/how-to-download-oracle-software-using-wget-or-curl/) to copy a valid cookies file from your Web browser to download Oracle Software from http://www.oracle.com

## How to copy a valid cookies file
If you don’t already have a tool to export the cookies from the Web browser to a text file, I’d suggest one of the browser extensions below:
- For Firefox: "Export Cookies" (https://addons.mozilla.org/en-US/firefox/addon/export-cookies/?src=api) 
- For Chrome: "cookies.txt export" (https://chrome.google.com/webstore/detail/cookietxt-export/lopabhfecdfhgogdbojmaicoicjekelh)

After installing the extension(s) above on the browser of your choice, follow the steps below:
- Initiate the download of the file you want (if downloading multiple files, you just need to do this for the first one)
- Once the download is initiated, cancel it.
- Export the cookies to a file (call it cookies.txt)
- If you’re using one of the extensions suggested above, this is how you do it:
 - On Firefox: click on Tools -> “Export cookies…” and save the file
 - On Chrome: click on the “cookies.txt export” icon in the toolbar (the icon is a blue “C” with an arrow inside), select the entire contents of the cookies and paste it into a text file.

Copy and Replace the cookies.txt file in the same directory as this Dockerfile.

It's necessary the copy a valid cookies file from your Web browser to download Oracle Software from http://www.oracle.com 

### Building WebLogic Images

To Build the WebLogic image, follow the steps below:
		- Go to folder /dockerfiles/12.1.3/wls

		- Run the following command:
			
			sudo docker build -t weblogic:12.1.3 .

		- Make sure you now have this image in place with
			
			sudo docker images




