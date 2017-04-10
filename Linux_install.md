### Installing osquery on Linux server

##### Building osquery from source
Osquery can also be compiled and installed from the source code. For this, you have to download the source from git repository.

###### Dependencies:
* sudo
* make [This has to be GNU make]
* python
* ruby
* git
* bash

Please note that default source package comes with command <code>make deps</code> to install or build above dependencies. Somehow, it also installs various packages that are not required to "**_use_**" osquery, else only required to build osquery binaries and packages on a host machine.

```
$ git clone http://github.com/facebook/osquery.git  
$ cd osquery  
$ make deps  
$ make -j 8  
$ ./build/<platform>/osquery/osqueryi
```


>_For reference page on osquery site, [**click here**](https://osquery.readthedocs.io/en/latest/development/building/)._

---
##### Instalation on Ubuntu/CentOS/RHEL machine

Apart from source compilation as mentioned above.
There are two additional methods to install osquery on Ubuntu/CentOS/RHEL Linux machine:
1. Using pre-built binary '.deb' or '.rpm' package. or;
2. from apt and yum repository

###### 1. From '.deb' or '.rpm' package.
You can download a pre-built osquery DEB or RPM package that contains osquery binaries, init.d script and example configurations.  
Using <code>dpkg -i {.deb package}</code> command in Ubuntu and <code>rpm -i {.rpm package}</code> command in CentOS/RHEL, you can install it in host machine.

* <b>Ubuntu 16.04 LTS Xenial</b>  
Download Link :  [xenial/osquery_2.3.2_1.16.amd64.deb](https://osquery-packages.s3.amazonaws.com/xenial/osquery_2.3.2_1.16.amd64.deb)  
Install Command:
<code>$ sudo dpkg -i /path/to/xenial/osquery_2.3.2_1.16.amd64.deb</code>  
* <b>Ubuntu 14.04 & 12.04 LTS</b>  
Download Link : [trusty/osquery_2.3.2_1.14.amd64.deb](https://osquery-packages.s3.amazonaws.com/trusty/osquery_2.3.2_1.14.amd64.deb)  
Install Command:
<code>$ sudo dpkg -i /path/to/trusty/osquery_2.3.2_1.14.amd64.deb</code>

* <b>CentOS/RHEL 7.0</b>  
Download Link :  [centos7/osquery-2.3.2-1.el7.x86_64.rpm](centos7/osquery-2.3.2-1.el7.x86_64.rpm)  
Install Command:
<code>$ sudo rpm -i /path/to/centos7/osquery-2.3.2-1.el7.x86_64.rpm</code>  
* <b>CentOS/RHEL 6.6</b>  
Download Link : [centos6/osquery-2.3.2-1.el6.x86_64.rpm](https://osquery-packages.s3.amazonaws.com/centos6/osquery-2.3.2-1.el6.x86_64.rpm)  
Install Command:
<code>$ sudo rpm -i /path/to/centos6/osquery-2.3.2-1.el6.x86_64.rpm</code>


###### 2. From 'apt' and 'yum' repository
There is no installable package for osquery in the official Ubuntu repository. You will have to add project repository to system.

<b>Ubuntu 16.04 LTS Xenial</b>  
```
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1484120AC4E9F8A1A577AEEE97A80C63C9D8B80B  
$ sudo add-apt-repository "deb [arch=amd64] https://osquery-packages.s3.amazonaws.com/xenial xenial main"  
$ sudo apt-get update  
$ sudo apt-get install osquery
```

<b>Ubuntu 14.04 & 12.04 LTS</b>  
```
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1484120AC4E9F8A1A577AEEE97A80C63C9D8B80B  
$ sudo add-apt-repository "deb [arch=amd64] https://osquery-packages.s3.amazonaws.com/trusty trusty main"  
$ sudo apt-get update  
$ sudo apt-get install osquery
```

<b>CentOS/RHEL 7.0</b>  
```
$ sudo rpm -ivh https://osquery-packages.s3.amazonaws.com/centos7/noarch/osquery-s3-centos7-repo-1-0.0.noarch.rpm  
$ sudo yum install osquery
```  

<b>CentOS/RHEL 6.6</b>  
```
$ sudo rpm -ivh https://osquery-packages.s3.amazonaws.com/centos6/noarch/osquery-s3-centos6-repo-1-0.0.noarch.rpm  
$ sudo yum install osquery
```

>_For download page on osquery site, [**click here**](https://osquery.io/downloads/)._

---
### Creating osquery configuration file
Creating configuration file makes it easier to run <code>osqueryi</code> and <code>osqueryd</code>. Insted of passing lots of command line options, both <code>osqueryi</code> and <code>osqueryd</code> can read those options from a configuration file located in <code>/etc/osquery/osquery.conf</code>.

<b>There are three sections to the configuration file:</b>  
* A list of daemon service options
* A list of scheduled queries to run and when they should run.
* A list of packs to include more specific scheduled queries.

Create and open the configuration file using following command:  
<code>$ sudo nano /etc/osquery/osquery.conf</code>

The configuration file uses the JSON format. Copy the following content into the file:  
```css
{
  "options": {
    "config_plugin": "filesystem",
    "logger_plugin": "filesystem",
    "logger_path": "/var/log/osquery",
    "disable_logging": "false",
    "log_result_events": "true",
    "schedule_splay_percent": "10",
    "pidfile": "/var/osquery/osquery.pidfile",
    "events_expiry": "3600",
    "database_path": "/var/osquery/osquery.db",
    "verbose": "false",
    "worker_threads": "2",
    "enable_monitor": "true",
    "disable_events": "false",
    "disable_audit": "false",
    "audit_allow_config": "true",
    "host_identifier": "hostname",
    "enable_syslog": "false",
    "audit_allow_sockets": "true",
    "schedule_default_interval": "3600"
  },
  "OSVer": {
    "crontab": {
      "query": "SELECT * FROM os_version;",
      "interval": 3600
    },
    "OSQSysProfile": {
      "query": "SELECT * FROM osquery_schedule;",
      "interval": 3600
    },
    "SysInfo": {
      "query": "SELECT * FROM system_info;",
      "interval": 3600
    }
  },
  "decorators": {
    "load": [
      "SELECT uuid AS host_uuid FROM system_info;",
      "SELECT user AS username FROM logged_in_users ORDER BY time DESC LIMIT 1;"
    ]
  },
  "packs": {
     "osquery-monitoring": "/usr/share/osquery/packs/osquery-dnif-linux.conf"
  }
}
```

Save and close the file, then validate it using the following command:  
<code>$ sudo osqueryctl config-check</code>  
If there's an error, the output will indicate the location of the error so you can fix it.

---  
### Running osqueryd
 <code>osqueryd</code> daemon allows osquery to run queries in a set intervals. Those queries include the ones you configured in above step.  
 Results generated by <code>osqueryd</code> are written to a file called <code>osqueryd.results.log</code> in the <code>/var/log/osquery directory</code>. By default this file won't exist. It only gets created when the daemon is started and starts generating results.

 You can start <code>osqueryd</code> using either <code>systemctl</code> or <code>osqueryctl</code>. Both accomplish the same thing, so it doesn't matter which one you use. osqueryd will check for the existence of a configuration file when it starts, and alert you if it doesn't find one. It will remain running without a configuration file, although it won't do anything useful.

 So, once the configuration file is setup as per above instructions. You can start daemon with following command:  
 <code>$ sudo systemctl start osqueryd</code>  
 Or you can type:  
 <code>$ sudo osqueryctl start</code>
