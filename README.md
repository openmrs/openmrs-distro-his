# OpenMRS Distro HIS

A sample software distribution that demonstrates how integration of OpenMRS with external systems can be achieved.


## Available commands

|Goal|Command|Explanation|
|:----|:----|:----|
|**Build** the distribution|<pre>./scripts/mvnw clean package</pre>|Assembles and packages your distribution, incorporating any configurations and customizations you've applied.|
|Access start/stop/destroy commands|<pre>source target/go-to-scripts-dir.sh</pre>|Navigates to the directory containing the scripts for starting, stopping, and destroying the distribution, making these commands readily accessible.|
|**Start** the distribution|<pre>./start-demo.sh</pre>|Initiates and launches all components of the Ozone HIS, bringing up the system.|
|**Stop** the distribution|<pre>./stop-demo.sh</pre>|Gracefully halts all Ozone HIS services, effectively shutting down the system.|
|**Destroy** the distribution|<pre>./destroy-demo.sh</pre>|Completely removes the distribution, clearing all its components and data, ideal for resetting the system or rectifying persistent issues ahead of a restart or a rebuild and restart.|
