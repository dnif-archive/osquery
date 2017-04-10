### OSQuery
It's an OS instrumental framework that can work with Linux, Windows, OS X(macOS) and FreeBSD platforms. It will perform a low-level monitoring and analytics on both performant and intuitive. It's a cross-platform application with support for recent versions of macOS, Windows 10, CentOS, and Ubuntu. It's officially described as an "SQL-powered operating system instrumentation, monitoring, and analytics" framework, and originated from Facebook.

###### It's a helpful tool to achieve:
* State-based change detection.
* File inteigrity monitoring
* Process auditing
* Socket auditing

###### Installing osquery gives you access to the following components:
* <code>osqueryi</code>: The interactive osquery shell, for performing ad-hoc queries.
* <code>osqueryd</code>: A daemon for scheduling and running queries in the background service mode.
* <code>osqueryctl</code>: A helper script for testing a deployment or configuration of osquery.  
It can also be used instead of the operating system's service manager to
start/stop/restart <code>osqueryd</code>.

[<code>osqueryi</code>](https://osquery.readthedocs.io/en/latest/introduction/using-osqueryi/) and [<code>osqueryd</code>](https://osquery.readthedocs.io/en/latest/introduction/using-osqueryd/) are indipendernt tool. They won't intercomunicate, and you can use one without impacting the other. Whereas, most of [command-line-flags](https://osquery.readthedocs.io/en/latest/installation/cli-flags/) and options need to run on each are same, and you can run <code>osqueryi</code> using <code>osqueryd</code>'s configuration file inorder t costomize the launch environment without using lots of command-line-switches and flags.

###### To start with osquery, you need:  
* osquery installed on a host machine.
* logging infrastructure <b>[dnif:Adapter component]</b>.
* Log forwarder installed on a host machine.

#### Installing osquery on the server:
* [Installation on Windaows server.][Link_to_win_install]
* [Installation on Linux server.][Link_to_Linux_install]

[Link_to_win_install]: https://github.com/gauravtango/osquery/blob/master/Windows_install.md
[Link_to_Linux_install]: https://github.com/gauravtango/osquery/blob/master/Linux_install.md

###### Osquery official helpful links:
* [Qsquery webpage](https://osquery.io)
* [Downlaod page link](https://osquery.io/downloads/)
* [Installation on Linux](https://osquery.readthedocs.io/en/latest/installation/install-linux/)
* [Installation on Windows](https://osquery.readthedocs.io/en/latest/installation/install-windows/)
* [Building from source code package](https://osquery.readthedocs.io/en/latest/development/building/)
* [osquery FAQ](https://osquery.io/faq/)
