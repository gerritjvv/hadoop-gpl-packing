
---



---



# News #
  * New release of version 0.3.0
    * contains latest 0.4.13 build of hadoop-lzo
    * contains latest 0.2 elephant bird releases.

# Issues #

Please report any issues or suggestions by clicking on the Issues tab and creating a new Issue

# Overview #
This project aims in making it easier for projects to use hadoop native compression binaries by providing rpm and deb installation packages that will distribute precompiled libraries for 32 and 64 bit architectures.

Various software projects would benefit from e.g. lzo compression but setup is a pain and error prone. Having an rpm or deb file to install makes life so much easier.

# Libraries included #
  * http://github.com/kevinweil/hadoop-lzo jar file
  * http://code.google.com/p/hadoop-gpl-compression lzo native bindinds
  * https://github.com/dvryaboy/elephant-bird
  * https://github.com/hirohanin/elephant-bird/
  * https://github.com/gerritjvv/elephant-bird

# How to use #

## Install ##
Download the rpm and install it on the machine to be used
The binaries will be installed in:
  * ` /opt/hadoopgpl/lib/*jar `
  * ` /opt/hadoopgpl/native `

## Using the binaries ##
The binaries include the lzo native bindings and kevin's github java hadoop code.

### Using in Hadoop ###

http://hadoop.apache.org/

Link the `/opt/hadoopgpl/native` directory to the `$HADOOP_HOME/lib/native/`

Or

Alternatively, you can define the java.library.path to point to one of the following:

  * /opt/hadoopgpl/native/Linux-amd64-64 for 64 bit platforms
  * opt/hadoopgpl/native/Linux-i386-32 for 32 bit platforms

Edit the hadoop configurion files (usally $HADOOP\_HOME/conf/mapred-site.xml) and change the property `mapred.child.java.opts` to include `-Djava.library.path=/opt/hadoopgpl/native/Linux-amd64-64` or `Linux-i386-32` depending on the architecture


---


### Using in Streams ###

http://code.google.com/p/bigstreams/

Run the following commands

---

**Agent**
  * mv ` /opt/streams-agent/lib/native/ /opt/streams-agent/lib/native-old `
  * ln -s `/opt/hadoopgpl/native /opt/streams-agent/lib/native `
  * ln -s `/opt/hadoopgpl/lib/hadoop-lzo.jar /opt/streams-agent/lib/hadoop-lzo.jar `
  * change the property ` writer.compressions.codec ` in the `/opt/streams-agent/conf/streams-agent.properties ` to ` writer.compressions.codec=com.hadoop.compression.lzo.LzopCodec`

---

**Collector**
  * ` rm -rf /opt/streams-collector/lib/native/ `
  * ` ln -s /opt/hadoopgpl/native /opt/streams-collector/lib/native `
  * change the property ` writer.compressions.codec ` in the `/opt/streams-collector/conf/streams-collector.properties ` to ` writer.compressions.codec=com.hadoop.compression.lzo.LzopCodec`

---


### Using in Pig ###

  * 1. A quick way is to include the line below in the `$PIG_HOME/bin/pig` file

  * ` PIG_OPTS="$PIG_OPTS -Djava.library.path=/opt/hadoopgpl/native/Linux-amd64-64" ` for 64 bit machines
  * ` PIG_OPTS="$PIG_OPTS -Djava.library.path=/opt/hadoop/lib/native/Linux-i386-32"` for 32 bit machines

  * 2. Add the following to ` $PIG_HOME/conf/pig.properties `
    * ` mapred.output.compression.codec=com.hadoop.compression.lzo.LzoCodec `
    * ` mapred.map.output.compression.codec=com.hadoop.compression.lzo.LzoCodec `
    * ` mapred.child.java.opts=-Djava.library.path=/opt/hadoop/lib/native/Linux-amd64-64 or Linux-i386-32 depending on the architecture. `

  * 3. Link or add the ` /opt/hadoopgpl/lib/hadoop-lzo-<version>.jar ` to a jar name in ` $PIG_HOME/lib `

  * 4. insert the code below into ` /opt/pig/bin/pig `
```
  # add hadoop lzo to CLASSPATH
  for f in $PIG_HOME/lib/hadoop*lzo*.jar; do
    CLASSPATH=${CLASSPATH}:$f;
   done

```

IMPORTANT: We need to do step 4 because of the name `hadoop-lzo.jar` pig tries in a clever way to add hadoop versions and will see the `hadoop-lzo.jar` as another hadoop jars and not add it automatically to the class path.


### Using elephantbird and Pig ###

The rpms are distributed with the elephant-bird flavors for pig:
| **pig version** | **path** | **git source** |
|:----------------|:---------|:---------------|
| 0.6.0 | ` /opt/hadoopgpl/lib/pig-0.6.0 ` | ` https://github.com/dvryaboy/elephant-bird ` |
| 0.7.0 | ` /opt/hadoopgpl/lib/pig-0.7.0 ` | ` https://github.com/dvryaboy/elephant-bird ` |
| 0.8.0 | ` /opt/hadoopgpl/lib/pig-0.8.0 ` | ` All three current elephant bird branches dvryaboy, hirohanin, gerritjvv` |

To use elephant bird the following libraries are included:
| **jar** | **path** |
|:--------|:---------|
| protobuf-java-2.3.0.jar | ` /opt/hadoopgpl/lib ` |
| slf4j-log4j12-1.5.10.jar | ` /opt/hadoopgpl/lib ` |
| yamlbeans-0.9.3.jar | ` /opt/hadoopgpl/lib ` |
| google-collect-1.0.jar | ` /opt/hadoopgpl/lib ` |


# How to Contribute #

Please send suggestions, additions on how to's, corrections etc to the streams user group mail

# Support and Contribution #
Please email the streams user group [streams-user@googlegroups.com] for support, suggestions,