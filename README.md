# Workaround for HADOOP-7682 on Windows: taskTracker could not start because "Failed to set permissions" to "ttprivate to 0700" 

This simple patch for Hadoop 1.0.3 was suggested by Joshua Caplan:
https://issues.apache.org/jira/browse/HADOOP-7682?page=com.atlassian.jira.plugin.system.issuetabpanels:comment-tabpanel&focusedCommentId=13440120#comment-13440120

This patch comprises a single class with two overridden methods that ignore `IOException`s when trying to set file permissions. I don't recommend running this patch in [production on Windows](http://en.wikisource.org/wiki/User:Fkorning/Code/Hadoop-on-Cygwin), or in an environment that supports full file permissions.


## Usage instructions

1. Download the pre-built JAR, `patch-hadoop_7682-1.0.x-win.jar`, from the [Downloads section](https://github.com/congainc/patch-hadoop_7682-1.0.x-win/downloads) (or build it yourself).
2. Copy `patch-hadoop_7682-1.0.x-win.jar` to the `${HADOOP_HOME}/lib` directory
3. Modify `${HADOOP_HOME}/conf/core-site.xml` to enable the overriden implementation as shown below:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!-- Put site-specific property overrides in this file. -->
        <configuration>
        	<property>
        		<name>fs.file.impl</name>
        		<value>com.conga.services.hadoop.patch.HADOOP_7682.WinLocalFileSystem</value>
        		<description>Enables patch for issue HADOOP-7682 on Windows</description>
        	</property>
        </configuration>

4. Run your job as usual. You should see some logging if the patch is enabled and working.

## Build instructions

This project is a NetBeans 7.x Ant-based project, which means you can also build it on the command line with a modern version of Ant.

The project was built for Hadoop 1.0.3. At minimum, you will need to specify the `${var.hadoop.home}` property in your Ant environment in order to build. If you're running a different version of Hadoop, you can change the Hadoop Core JAR file name in `nbbuild/project.properties` as the value of the `file.reference.hadoop-core-1.0.3.jar` key (you shouldn't change the key name itself, as it's merely symbolic and arbitrary).
