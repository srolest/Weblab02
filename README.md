# Weblab 02: Autenticación Basic y Digest

This project consists of configuring an Apache web server on a virtualized infrastructure with Vagrant, 
implementing different authentication methods (Basic and Digest) and group access control.

## Infrastructure
The network uses the address `192.168.56.0/24`:
**dns.sistema.sol** (`192.168.56.100`): Master DNS server for the domain `sistema.sol`.
**tierra.sistema.sol** (`192.168.56.101`): Apache web server.
**discovery.sistema.sol**: Alias (CNAME) configured for authentication testing.

## Configuration
### 1. Web Server (Virtual Host)
The configuration file `discovery.sistema.sol.conf` has been created with the following access control:
**Directory `/basic`**: Basic authentication enabled for all valid users.
**Directory `/basic/development`**: Access restricted to the `development` group (user: `ana`).
**Directory `/basic/sales`**: Access restricted to the `sales` group (user: `arturo`).
**Directory `/digest`**: Digest authentication for the user `commander` in the `astronauts` realm.

Below is a screenshot of the configuration file:
[CONFIGURATION_FILE](./images/configurationfile.png)

### 2. User Management and Security
The credential files are located in the `/etc/apache2/` path on the `tierra` server:
`.htpasswd_basic`: Contains the hashes for Ana, Maria, and Arturo.
`.htgroups`: Defines the sales and development groups.
`.htpasswd_digest`: Stores the digest credential for the commander user.

I have also configured the vagrantfile to do everything automatically by taking the files from .vagrant.
[VAGRANTFILE](./images/vagrantfile.png)
## Verification
To validate the configuration, the **HURL** tool was used with the `weblab-2.hurl` file.

### Running the tests:
```bash
hurl --test --variable site=discovery.sistema.sol weblab-2.hurl
``` 

### Test results:
```bash
serafinroldan@serafin-hplaptop:~/Escritorio/2ASIR/SRI/Weblab02$ hurl --test --variable site=discovery.sistema.sol weblab-2.hurl
weblab-2.hurl: Running [1/1]
weblab-2.hurl: Success (11 request(s) in 7 ms)
--------------------------------------------------------------------------------
Executed files:  1
Succeeded files: 1 (100.0%)
Failed files:    0 (0.0%)
Duration:        8 ms
``` 

Since Hurl does not verify the digest authentication method, we do so by entering the URL with the domain in the browser: 
**discovery.sistema.sol/digest**.
We can see how it asks us for the username and password we have configured. Below are some screenshots of the process.
[DIGEST_TEST](./images/digest_test1.png) | [DIGEST_TEST](./images/digest_test2.png)