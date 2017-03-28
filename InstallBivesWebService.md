Install the BiVeS Web Service 
===============================

To install the BiVeS Web Service you need to have the latest binaries:
* see  [Build BiVeS WebService](BuildBivesWebService)
* or download from [http://bin.sems.uni-rostock.de/BiVeS-WS/](http://bin.sems.uni-rostock.de/BiVeS-WS/)

Installation on on a Tomcat Web Server 
---------------------------------------

You need to have Tomcat installed. Since the installation differs depending on the operating system you're using we cannot provide you with detailed installation instructions, but a [web search](https://www.google.com/search?q=install+tomcat) will probably guide you through the installation process. If you nevertheless have trouble installing Tomcat feel free to [contact one of us](https://sems.uni-rostock.de/people/).

If Tomcat is installed on your system just deploy (copy) the binary to you `$CATALINA_BASE/webapps` directory (on Debian based systems you'll find it in `/var/lib/tomcat7/webapps` by default). If that is done the tomcat logs will contain some messages like the following:

```
Mar 17, 2014 9:00:27 PM org.apache.catalina.startup.HostConfig deployWAR
INFO: Deploying web application archive /var/lib/tomcat7/webapps/BiVeS-WS-1.2.3.war
```

If that is done the web service will hopefully be up and running at http://you.server:8080/BiVeS-WS-1.2.3

### Rename the Package 

Since it might be annoying to have the version number of the web service in its URL you can simply rename the `war` package. E.g. copy `BiVeS-WS-1.2.3.war` to `$CATALINA_BASE/webapps/bives-rocks.war` to get it accessible at http://you.server:8080/bives-rocks

### Get rid of 8080 

You may want to get rid of the `:8080` in the BiVeS URL. This is because the Tomcat web server by default binds to port 8080 on your web server. If you're not running another web server at the default port for HTTP web servers (`:80`) you can simply configure Tomcat to bind to `:80`.

If there is another web server on `:80` there are some workarounds to integrate Tomcat (see e.g. [Integrating Tomcat with Apache](http://binfalse.de/2013/10/integrating-tomcat-with-apache/))