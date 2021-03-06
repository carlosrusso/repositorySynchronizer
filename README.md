repositorySynchronizer
===
Pentaho Plugin allowing synchronization between JCR and the File System.

PRS depends on the following CTools:

* CDF: Community Dashboard Framework
* CDE: Community Dashboard Editor
* CDA: Community Data Access
* CGG: Community Graphics Generator
* 
You can install these packages from the Pentaho Marketplace, or manually download them from [Pentaho's Continuous Integration server](http://ci.pentaho.com) and unzip the .zip file to your `pentaho-solutions/system` folder.

If you are using the Enterprise Edition, we recommend you to install the Pentaho Marketplace. Follow the instructions at [Pedro Alves's blog](http://pedroalves-bi.blogspot.pt/2013/11/ctools-for-pentaho-50-is-available-cdf.html).

### Configuration

By default, the Pentaho Repository Synchronizer stores its settings in the file `prs.xml`, which by default looks like this:

    <?xml version="1.0" encoding="UTF-8"?>
    <prs>
    	<locations>
    		<location path="jcr-solution:http://127.0.0.1:8080/pentaho!/" name="JCR"/>
    		<location path="${cpk.solution.system.dir}/../repositorySynchronizer" name="File System"/>
    	</locations>
    	<excludes>
    		<!-- Exclude all dot folders and files -->
    		<exclude pattern=".*/\.[^/]*(/.*)?"/>
    		<!-- Exclude sample folders -->
    		<exclude pattern=".*/bi-developers(/.*)?"/>
    		<exclude pattern=".*/pentaho-operations-mart(/.*)?"/>
    		<exclude pattern=".*/plugin-samples(/.*)?"/>
    		<exclude pattern=".*/Steel\sWheels(/.*)?"/>
    	</excludes>
    	<last_synch>0</last_synch>
    </prs>

Note that:

 * the variable `${cpk.solution.system.dir}` points to `pentaho-solutions/system`
 * by default, the sample files distributed with a pristine installation are excluded. Dot files such as `.git` are also ignored.


## Pro tips
### Access JCR from within Spoon
Developing PDI jobs and transformations that manipulate files in the JCR can be a bit hard given the fact that Spoon does not allow you to access the JCR out of the box.

In fact, Spoon requires some jar files to be installed in order to be able to operate with a JCR repository running on a BI server.
Here's what you need to do:


1. Copy the file `libpensol-TRUNK-SNAPSHOT.jar` from `biserver/pentaho-solutions/system/repositorySynchronizer/_lib_`
into `data-integration/lib`.
2. Copy the file `pentaho-platform-repository-5.*-stable.jar` from 
`biserver/tomcat/webapps/pentaho/WEB-INF/lib` into `data-integration/lib`.

Now you can access the files in the JCR by using a root URI such as `jcr-solution:http://user:password@127.0.0.1:8080/pentaho!/`. Note that from within Spoon, the URI needs to contain the credentials to access the server.
