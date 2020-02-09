# Syntax verification of YAML in Playbooks
- Syntax errors in playbook cause execution to fail

- Execution output may or may not help pinpoint source of syntax error

- Best practice: Verify YAML syntax in playbook prior to execution

- Several ways to do this

## Python
To read playbook YAML file using Python, use command similar to:

`python -c 'import yaml, sys; print yaml.load(sys.stdin)' < myyaml.yml`