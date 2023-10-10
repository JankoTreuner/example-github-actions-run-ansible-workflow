# example-github-actions-run-ansible-workflow
Example showing how to run an ansible playbook using github actions

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

### Github action workflow
See file ``` .github/workflows/main.yml ``` 


## Credits
While looking for an example how to use github actions to run ansible notebooks I stumbled over the following article of @xNok (https://github.com/xNok) at medium:
https://medium.com/faun/how-to-run-an-ansible-playbook-using-github-action-42430dec944
This repository contains the outcome of playing around with this example as a start.