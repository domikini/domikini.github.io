# Syntax verification of YAML in Playbooks
- Syntax errors in playbook cause execution to fail

- Execution output may or may not help pinpoint source of syntax error

- Best practice: Verify YAML syntax in playbook prior to execution

- Several ways to do this

## Python
To read playbook YAML file using Python, use command similar to:

`python -c 'import yaml, sys; print yaml.load(sys.stdin)' < myyaml.yml`


## YAML Lint
-  Online YAML syntax verification tools available
   
-  Useful for administrators not familiar with Python
   
-  Example: YAML Lint website: http://yamllint.com/

## Syntax-check
- Ansible offers native feature for validating playbook YAML syntax

- Use --syntax-check option with ansible-playbook command to check for syntax errors

- --syntax-check method:

- Conducts more rigorous review

- Ensures data elements specific to playbooks are not missing

- Recommended for verifying playbook YAML syntax

