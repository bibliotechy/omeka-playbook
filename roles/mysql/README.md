MYSQL role
==========

[![Build Status](https://travis-ci.org/lyrasis/ansible-mysql-role.svg?branch=master)](https://travis-ci.org/lyrasis/ansible-mysql-role)

Install a MySQL server.

Considerations
--------------

If binding to host other than localhost will need to setup the firewall accordingly. For example:

```
ufw allow from 10.11.12.15 proto tcp to any port 3306
```

Variables
---------

Role variables can be added to (as needed):
- group_vars
- host_vars
- *.yml

List of variables:

<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Type</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>mysql_bind_addr</td>
      <td>string</td>
      <td>Addresses the server will listen to i.e. localhost or 0.0.0.0</td>
    </tr>
      <td>mysql_max_connections</td>
      <td>integer</td>
      <td>No. of client connections</td>
    </tr>
    <tr>
      <td>mysql_root_pass</td>
      <td>string</td>
      <td>Root user password</td>
    </tr>
    <tr>
      <td>mysql_buffer_pool_size</td>
      <td>string</td>
      <td>InnoDB buffer pool RAM allocation</td>
    </tr>
    <tr>
      <td>mysql_database</td>
      <td>array of hashes</td>
      <td>The databases and user / passwords to create</td>
    </tr>
  </tbody>
</table>

---

Example database variable (creates two databases, and users for each database with a password and hosts specified):

```
mysql_database:
  -
    name: alpha
    user: alpha
    pass: alpha
    hosts: 
      - "%"
  -
    name: alpha
    user: alpha_reports
    pass: alpha_reports
    priv: "SELECT"
    hosts:
      - "%"
  -
    name: beta
    user: beta
    pass: beta
    hosts: 
      - 127.0.0.1
  -
    name: beta
    user: beta_reports
    pass: beta_reports
    priv: "SELECT"
    hosts:
      - "%"
```

If priv is not specified the default is: `ALL`

---
