nfb-galaxy.ssh-key
========

Grant access from the host to another server (outbound) using a pre-provisioned ssh private key, or
grant access to a certain user account on the host (inbound) using a pre-provisioned ssh public key.

Requirements
------------

ssh is required for this module to be useful.

Role Variables
--------------

Specify either 'outbound' or 'inbound' variant:

    # Use 'outbound' to enable outbound ssh connections (from the host machine to another).
    outbound:
      user: ""     # The user account where the private key will be installed. Leave empty to use the "current" user.
      private_key:
       file: ""    # Path to the private key file
       name: ""    # The name that will be given to the key file on the host machine.
      hosts: []    # A list of hosts for which the private key will be used to connect to.
    
    # Use 'inbound' to enable inbound ssh connections to a user account (from a remote machine to the host).
    inbound:
      user: ""        # The user account to which the remote machines will be granted access.
      public_key: ""  # The ssh public key that will be used for authentication.


Dependencies
------------

N/A

Example Playbook
-------------------------

Example of an outbound setup to allow the user "bob" to clone projects from the git servers:

    - hosts: servers
      roles:
         - role: nfb-galaxy.ssh-key
           outbound:
             user: "bob"
             private_key: {file: "../keys/git/id_rsa", name: "git_id_rsa"}
             hosts:
               - git1.localnet.com
               - git2.localnet.com

Example of an inbound setup to allow a remote machine to connect to bob's account via ssh:

    - hosts: servers
      roles:
         - role: nfb-galaxy.ssh-key
           inbound:
             user: "bob"
             public_key: "../keys/id_rsa.pub"
