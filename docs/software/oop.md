# Object Oriented Programming
- Encapsulation
- Abstraction
- Inheritance
- Polymorphism

---
## Java

### Install JDK

#### Debian / Ubuntu
`apt-get install openjdk-11-jdk`

### CentOS / Fedora / RHEL
`yum -y install java-11-openjdk java-11-openjdk-devel`
```
cat > /etc/profile.d/java11.sh <<EOF
export JAVA_HOME=$(dirname $(dirname $(readlink $(readlink $(which javac)))))
export PATH=\$PATH:\$JAVA_HOME/bin
EOF
```
`source /etc/profile.d/java11.sh`

#### Windows
- [OpenJDK](https://openjdk.java.net/install/)
    - [Install page for JDK 11](http://jdk.java.net/11/)
    - [OpenJDK Archive](http://jdk.java.net/archive)

1. Download JDK
2. Set `PATH` (e.g. add `.../Java/jdk-11/bin` to `PATH`)
3. Set `JAVA_HOME` (i.e. set to jdk installation path without `/bin` dir)
4. Test with `java --version` command and configure the JDK in IntelliJ