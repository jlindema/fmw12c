WebLogic on Docker
===============
Docker configurations to facilitate installation, configuration, and environment setup for developers. This project includes [dockerfile](/images/12.1.3/soa) for SOA 12c 12.1.3 .

## Based WebLogic 12c (12.1.3) images
For more information please read the [WebLogic 12c Docker Images](/images/12.1.3/wls) page.
The WebLogic 12c images must be built with the name "fmw12c/wls:12.1.3"


## How to build and run
This project offers Dockerfile for SOA 12c (12.1.3) to download and to build image with the following packages:
- fmw_12.1.3.0.0_infrastructure.jar
- fmw_12.1.3.0.0_osb.jar
- fmw_12.1.3.0.0_soa.jar
- fmw_12.1.3.0.0_mft.jar
To download the packages from the Oracle Software Delivery Cloud Site (edelivery.oracle.com), it's necessary to be authenticate in the site.
To do this, I use the method [Export cookies](http://www.pythian.com/blog/how-to-download-oracle-software-using-wget-or-curl/) to copy a valid cookies file from your Web browser to download Oracle Software from https://edelivery/oracle.com

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

It's necessary the copy a valid cookies file from your Web browser to download Oracle Software from https://edelivery.oracle.com 

### Building WebLogic Images

To Build the WebLogic image, follow the steps below:

- Go to folder /images/12.1.3/soa

- Run the following command:
 
        sudo docker build -t fmw12c/soa:12.1.3 .

- Make sure you now have this image in place with

        sudo docker images




