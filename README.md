# Ansible Matrix Server Role  
## Summary
__Graph:__
``` mermaid
graph LR;
  subgraph Roles
    common_role-->matrix_role;
    common_role-.->caddy_role;
  end
  subgraph Hosts
    matrix_role-->Matrix;
    caddy_role-.->Caddy;
    Router-.Ports 80,8448.->Caddy;
    Caddy-.Port 8008.->Matrix;
  end
```
This roles setups up a Matrix chat server (this does not include the element web app). When a caddy reverse proxy is present in the ansible inventory file the reverse proxy file will be pushed to caddy and caddy will be allowed access to the matrix server. 

## Requirements
### ansible-galaxy
__Collections:__
  - [community.general](https://docs.ansible.com/ansible/latest/collections/community/general/index.html)

__Roles:__
  - [daemonslayer2048.common](https://github.com/Daemonslayer2048/common_role)

## Variables
| Variable | Type | Required | Default | Example |
|    -     |   -  |     -    |    -    |    -    |
| [hostname](#hostname) | string | True | N/A | cloud |
| [host_public_domain](#host_public_domain) | string | True | N/A | null.com |
| [matrix_user](#matrix_user) | string | True | matrix | matrix |
| [matrix_user_home](#matrix_user_home) | string | True | /srv/matrix | /srv/matrix |
| [psql_application_user](#psql_application_user) | string | True | N/A | Matrix |
| [psql_application_password](#psql_application_password) | string | True | N/A | Password1 |
| [psql_application_database](#psql_application_database) | string | True | N/A | Matrix |

### Variable Summaries:
#### hostname
This is used in multiple locations to set the FQDN of the server and to be used in setting the public URL for the server when the [Caddy reverse proxy role](https://github.com/Daemonslayer2048/caddy_role) is deployed.

#### host_public_domain
Completes the domain portion of the public URL with the help of the variable above.

#### matrix_user  
The name of the user matrix will run as on the host

#### matrix_user_home  
The home of the matrix service user

#### psql_application_user  
The username the matrix service will use to authenticate to the database.

#### psql_application_password  
The password the matrix service will use to authenticate to the database.

#### psql_application_database  
The database name the matrix service will use.


## Notes:
### Testing Issues
  - Selinux Modules can not be tested in podman so these tests are performed outside of the stesting suite :(
