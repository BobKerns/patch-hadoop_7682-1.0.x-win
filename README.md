# Workaround for HADOOP-7682

This patch was suggested by Joshua Caplan:
https://issues.apache.org/jira/browse/HADOOP-7682?page=com.atlassian.jira.plugin.system.issuetabpanels:comment-tabpanel&focusedCommentId=13440120#comment-13440120

## Usage instructions

1. Copy the built JAR to the ${HADOOP_HOME}/lib directory
2. Modify ${HADOOP_HOME}/conf/core-site.xml
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


## Build instructions
