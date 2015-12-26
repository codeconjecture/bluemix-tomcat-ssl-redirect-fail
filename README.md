
#Tomcat 8 Hello World with global HTTP -> HTTPS redirect

There may be a significant IBM Bluemix infrastructure configuration issue on the load balancing side for Cloud Foundry Java Community Buildpacks.  IBM Bluemix support team, I would greatly appreciate some feedback.

I am using IBM Bluemix with the Java Community Buildpack (not Liberty), which by default uses Tomcat.  I am trying to enable all redirects from http to https, but keep getting a "Too Many Redirects" error message when visiting the webapp in a browser.

Here is the relevant **web.xml** portion

    <security-constraint>
		<web-resource-collection>
			<web-resource-name>Wildcard means whole app requires authentication</web-resource-name>
			<url-pattern>/*</url-pattern>
			<http-method>GET</http-method>
			<http-method>POST</http-method>
		</web-resource-collection>

		<user-data-constraint>
			<transport-guarantee>CONFIDENTIAL</transport-guarantee>
		</user-data-constraint>
	</security-constraint>

The Java Community Buildpack **server.xml**, https://github.com/cloudfoundry/java-buildpack/blob/master/resources/tomcat/conf/server.xml, already references:

    <Valve className="org.apache.catalina.valves.RemoteIpValve" protocolHeader="x-forwarded-proto"/>

The best IBM BLuemix link I could find on this is:  https://developer.ibm.com/answers/questions/16016/how-do-i-enforce-ssl-for-my-bluemix-application.html, but this does not address Java Community Buildpacks.  Furthermore, you'll see that this post mentioned that the proper header is indeed set, "X-Forwarded-Proto"

Any help would be appreciated.  Very time sensitive requirement.  

Here is a github with the sample app being tested,
https://github.com/codeconjecture/bluemix-tomcat-ssl-redirect

Here is the same app failing on Bluemix:  http://tomcatredirect.mybluemix.net/hello.jsp
(too many redirects error)
https://tomcatredirect.mybluemix.net/hello.jsp
(too many redirects error)

Here is the same app working on Pivotal CF:
http://tomcatredirect.cfapps.io/hello.jsp
(redirect to https)
https://tomcatredirect.cfapps.io/hello.jsp
(works on https)

After checking out, you can push the code with
    cf push <testappname> -p test.war -b https://github.com/cloudfoundry/java-buildpack

Links where this question has been asked:
http://stackoverflow.com/questions/34420203/simple-java-helloworld-webapp-redirect-http-https-fails-on-ibm-bluemix-but-works
https://developer.ibm.com/answers/questions/245322/how-to-redirect-http-to-https-on-ibm-bluemix-using.html
