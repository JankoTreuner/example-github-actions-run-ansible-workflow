# example-github-actions-run-ansible-workflow
Example showing how to run an ansible playbook using github actions

## How to run this example (in a forked repository, with your own configuration)
Setup GitHub secrets INVENTORY, KNOWN_HOSTS and SSH_KEY.


> [!IMPORTANT]  
> Ensure that the tool you use to set the GitHub secret supports multiline content (VSCode seems not to support it). 
> I recommend using the github.com website itself.

Afterwards every push to the repository or manual trigger of the workflow direktly (via GitHub.com -> Actions) should trigger the workflow and execute the playbook on the confiured hosts.

For more details see description below.

## How this example works

This repository contains following things:
- python requirements for running ansible
- simple ansible playbook 
- github action workflow

### Python requirements for running ansible
See file ``` requirements.txt ```

Install requirements locally using a venv:

``` bash
python3 -m venv ./venv
source venv/bin/activate
pip install -r requirements.txt
```

### Simple ansilbe-playbook
See file ``` ansible/hello-world.yaml ``` 

This playbooks writes a file ``` /tmp/testfile.txt``` with the content ``` hello-world ```

### Github action workflow
See file ``` .github/workflows/main.yml ``` 

The workflow contains the following jobs
1. validate - Validates the playbook, using ansible-lint
2. run-playbook

#### validate
Runs asible-lint to ensure the code quality of the playbook.

#### run-playbook

##### Steps

###### install requirements
Installs requirements.txt or test.requirements.txt if file exists in project-root.

###### write inventory to file
Writes the inventory information from GitHub secret to file

The content should be a valid ansible inventory

``` yaml
ungrouped:
  hosts:
    my.domain.de: 
      ansible_user: the_user
```

###### configure ssh
Creates everything need for ssh. Therefor GitHub secret KNOWN_HOSTS and SSH_KEY must exist.

*KNOWN_HOSTS*
Well known file ~/.ssh/known_hosts. The GitHub secret must be set to the required hosts.
```
my.domain.de ecdsa-sha2-nistp256 <key>
```

*SSH_KEY*
The secret SSH_KEY must be set to a valid private key for the user that is used to run ansible on the target.

1. generate ssh-key
``` bash 
ssh-keygen -o -a 1000 -t ed25519
```

2. add public key to the _authorized_keys_ of the user on the target system.

3. set the private key as GitHub secret SSH_KEY.
Should be something like:
``` 
-----BEGIN OPENSSH PRIVATE KEY-----
<LongPrivateKey>...==
-----END OPENSSH PRIVATE KEY-----
```


## Credits
While looking for an example how to use github actions to run ansible notebooks I stumbled over the following article of @xNok (https://github.com/xNok) at medium:
https://medium.com/faun/how-to-run-an-ansible-playbook-using-github-action-42430dec944
This repository contains the outcome of playing around with this example as a start.