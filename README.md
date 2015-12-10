### Index
1. Files in this tool
2. How this tool works
3. Limitation
4. Example usage
-------

Files in this tool
======

- pom.xml
   A normal maven Pom which contains dependencies you need to download
- settings.xml
   A maven settings file which defined remote repository you need to download from.
   Here we defined 2 maven repo, Maven Central by maven.org and Jboss Central by redhat. In most case, these 2 remote repo is enough.
- README.txt

   
How this tool works
======

This is a normal maven project but without any source code inside.
By running maven command "mvn dependency:go-offline", the jars defined in pom.xml and their dependency jars will be resolved by maven and download from remote repository to "target" directory.

```code
mvn dependency:go-offline --settings=./settings.xml -Dmaven.repo.local=target
  * dependency:go-offline :Download all dependency to target
  * --settings=./settings.xml  :Use local maven setting
  * -Dmaven.repo.local=target  :Define target directory name
```

After that you can copy "target" directory to your offline central repository like Nexus or Artifactory.


Limitation
======

This tool will download not only jars defined in pom.xml, but also download all jars used be maven standard plug-in, like maven-dependency-plugin and their dependency. This behavior will take more time than you expected, but no harm to your local repository.


Example usage
======

1. make sure your target directory is not exists. If exist, delete it.
2. make sure your command prompt is at same directory as pom.xml and settings.xml.
3. run following command will download jars and copy them to nexus hosted repository.
4. Finally, login to Nexus web console and click "Repair Index" of nexus hosted repository.

```sh
export Nexus_Local_Storage_Location=~/jboss/vagrant/install/nexus-repo/others
rm -r target
mvn dependency:go-offline --settings=./settings.xml -Dmaven.repo.local=target
cp -R ./localrep/ ${Nexus_Local_Storage_Location}/
```