Description
===========

#### Production

This stack is running the latest version of Galera 10.1.


Instructions
===========

#### Getting Started
If you're new to MySQL or Galera, the [MySQL Manual](http://dev.mysql.com/doc/refman/5.6/en/index.html)
documentation will step you through the basics of configuration, connecting,
user, and permission management, and the [Galera Manual](http://galeracluster.com/documentation-webpages/).

By default, the "root" MySQL user can only connect from localhost (127.0.0.1).
You will need to connect to the server and use the MySQL client in order to
allow "root" to connect from external servers.

As a part of your server configuration, your server will be configured to run
nightly backups leveraging
[Holland](https://github.com/holland-backup/holland#readme).  Backups will be
stored in the directory /var/lib/mysqlbackups directory on the master node.
#### Logging in via SSH
The private key provided in the passwords section can be used to login as
root via SSH. We have an article on how to use these keys with [Mac OS X and
Linux](http://www.rackspace.com/knowledge_center/article/logging-in-with-a-ssh-private-key-on-linuxmac)
as well as [Windows using
PuTTY](http://www.rackspace.com/knowledge_center/article/logging-in-with-a-ssh-private-key-on-windows).
#### Additional Notes
All write operations should be performed on the Master node. Read operations
can be performed against any servers in this deployment. By default, all new
and existing databases will be replicated across the members of this
deployment.
All write or read operations can be performed against any nodes in the
cluster.  Additionally, you can add or remove nodes at will by adjusting the
"server_count" parameter.  Please note that, after adding a new node to the
cluster, it can take some time for the data to be synchronized to the new
node.


Requirements
============
* A Heat provider that supports the following:
  * OS::Heat::RandomString
  * OS::Heat::ResourceGroup
  * OS::Heat::SoftwareConfig
  * OS::Heat::SoftwareDeployment
  * OS::Heat::SoftwareDeploymentGroup
  * OS::Nova::KeyPair
  * OS::Nova::Server
* An OpenStack username, password, and tenant id.
* [python-heatclient](https://github.com/openstack/python-heatclient)
`>= v0.2.8`:

```bash
pip install python-heatclient
```

We recommend installing the client within a [Python virtual
environment](http://www.virtualenv.org/).

Parameters
==========
Parameters can be replaced with your own values when standing up a stack. Use
the `-P` flag to specify a custom parameter.

* `galera_user`: User to create within Galera (Default: galera_user)
* `galera_database`: Database to create within Galera.  User defined in "Galera User" will have full access to this database (Default: galera_db)
* `server_flavor`: Flavor of Cloud Server to be used for all servers in this stack (Default: 4 GB General Purpose v1)
* `server_count`: Number of secondary web nodes (Default: 2)
* `server_image`: Image to use for WordPress (Default: f4bbbce2-50b0-4b07-bf09-96c175a45f4b)
* `secondary_template`: Template to use for secondary servers (Default: https://raw.githubusercontent.com/rackspace-orchestration-templates/galera/master/galera-secondary.yaml)
* `ansible_branch`: The Ansible Roles will be pulled from Git, using the tag or branch provided (Default: stable)
* `ansible_repo`: The Ansible Roles will be pulled from Git, using the repo provided (Default: https://github.com/rackspace-orchestration-templates/ansible-roles.git)

Outputs
=======
Once a stack comes online, use `heat output-list` to see all available outputs.
Use `heat output-show <OUTPUT NAME>` to get the value of a specific output.

* `galera_public_ip`: Master IP 
* `galera_password`: Galera Root Password 
* `galera_user_password`: Galera Password 
* `galera_user`: Galera User 
* `ssh_private_key`: SSH Private Key 
* `secondary_ips`: Secondary Node IPs 

For multi-line values, the response will come in an escaped form. To get rid of
the escapes, use `echo -e '<STRING>' > file.txt`. For vim users, a substitution
can be done within a file using `%s/\\n/\r/g`.
