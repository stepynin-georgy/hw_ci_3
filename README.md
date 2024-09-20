# Домашнее задание к занятию 10 «Jenkins»

## Подготовка к выполнению

1. Создать два VM: для jenkins-master и jenkins-agent.
2. Установить Jenkins при помощи playbook.

```
(venv_ci) root@ansible-ubuntu:/opt/hw_ci_3# ansible-playbook -i infrastructure/inventory/cicd/hosts.yml infrastructure/site.yml
[DEPRECATION WARNING]: Ansible will require Python 3.8 or newer on the controller starting with Ansible 2.12. Current version: 3.6.15 (default, Sep 13 2024, 08:34:12) [GCC 13.2.0]. This feature
will be removed from ansible-core in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
/opt/venv_ci/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature

PLAY [Preapre all hosts] ****************************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************************************
ok: [jenkins-agent-01]
ok: [jenkins-master-01]

TASK [Create group] *********************************************************************************************************************************************************************************
changed: [jenkins-master-01]
changed: [jenkins-agent-01]

TASK [Create user] **********************************************************************************************************************************************************************************
changed: [jenkins-master-01]
changed: [jenkins-agent-01]

TASK [Install JDK] **********************************************************************************************************************************************************************************
changed: [jenkins-master-01]
changed: [jenkins-agent-01]

PLAY [Get Jenkins master installed] *****************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************************************
ok: [jenkins-master-01]

TASK [Get repo Jenkins] *****************************************************************************************************************************************************************************
changed: [jenkins-master-01]

TASK [Add Jenkins key] ******************************************************************************************************************************************************************************
changed: [jenkins-master-01]

TASK [Install epel-release] *************************************************************************************************************************************************************************
ok: [jenkins-master-01]

TASK [Install Jenkins and requirements] *************************************************************************************************************************************************************
changed: [jenkins-master-01]

TASK [Ensure jenkins agents are present in known_hosts file] ****************************************************************************************************************************************
# 84.252.138.100:22 SSH-2.0-OpenSSH_7.4
# 84.252.138.100:22 SSH-2.0-OpenSSH_7.4
# 84.252.138.100:22 SSH-2.0-OpenSSH_7.4
# 84.252.138.100:22 SSH-2.0-OpenSSH_7.4
# 84.252.138.100:22 SSH-2.0-OpenSSH_7.4
changed: [jenkins-master-01] => (item=jenkins-agent-01)
[WARNING]: Module remote_tmp /home/jenkins/.ansible/tmp did not exist and was created with a mode of 0700, this may cause issues when running as another user. To avoid this, create the remote_tmp
dir with the correct permissions manually

TASK [Start Jenkins] ********************************************************************************************************************************************************************************
changed: [jenkins-master-01]

PLAY [Prepare jenkins agent] ************************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************************************
ok: [jenkins-agent-01]

TASK [Add master publickey into authorized_key] *****************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Create agent_dir] *****************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Add docker repo] ******************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Install some required] ************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Update pip] ***********************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Install Ansible] ******************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Reinstall Selinux] ****************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Add local to PATH] ****************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Create docker group] **************************************************************************************************************************************************************************
ok: [jenkins-agent-01]

TASK [Add jenkinsuser to dockergroup] ***************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Restart docker] *******************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

TASK [Install agent.jar] ****************************************************************************************************************************************************************************
changed: [jenkins-agent-01]

PLAY RECAP ******************************************************************************************************************************************************************************************
jenkins-agent-01           : ok=17   changed=14   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
jenkins-master-01          : ok=11   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

/opt/venv_ci/bin/ansible-playbook:135: ResourceWarning: unclosed file <_io.BufferedRandom name=5>
  exit_code = cli.run()
```

3. Запустить и проверить работоспособность.
4. Сделать первоначальную настройку.

## Основная часть

1. Сделать Freestyle Job, который будет запускать `molecule test` из любого вашего репозитория с ролью.
2. Сделать Declarative Pipeline Job, который будет запускать `molecule test` из любого вашего репозитория с ролью.

```
Started by user admin
Running as SYSTEM
Building remotely on agent (linux ansible) in workspace /opt/jenkins_agent/workspace/freestyle
The recommended git tool is: NONE
using credential e04d7e0d-faee-40f4-9a9c-c6ea83fbb998
 > git rev-parse --resolve-git-dir /opt/jenkins_agent/workspace/freestyle/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/stepynin-georgy/vector-role.git # timeout=10
Fetching upstream changes from https://github.com/stepynin-georgy/vector-role.git
 > git --version # timeout=10
 > git --version # 'git version 1.8.3.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --progress https://github.com/stepynin-georgy/vector-role.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/tests^{commit} # timeout=10
Checking out Revision fe11e4c1dc125282e58c41cdd3b61d930a2ea841 (refs/remotes/origin/tests)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f fe11e4c1dc125282e58c41cdd3b61d930a2ea841 # timeout=10
Commit message: "Update converge.yml"
 > git rev-list --no-walk bf545af898e901a55973156ed2b8e1456b218d3e # timeout=10
[freestyle] $ /bin/sh -xe /tmp/jenkins4904487739187297082.sh
+ pip3 install -r tox-requirements.txt
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: selinux in /usr/local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 1)) (0.2.1)
Requirement already satisfied: ansible-lint==5.1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 2)) (5.1.3)
Requirement already satisfied: yamllint==1.26.3 in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 3)) (1.26.3)
Requirement already satisfied: lxml in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 4)) (5.3.0)
Requirement already satisfied: molecule==3.4.0 in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 5)) (3.4.0)
Requirement already satisfied: molecule_docker in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 6)) (1.1.0)
Requirement already satisfied: jmespath in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 7)) (0.10.0)
Requirement already satisfied: wcmatch>=7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (8.3)
Requirement already satisfied: tenacity in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (8.2.2)
Requirement already satisfied: pyyaml in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (5.4.1)
Requirement already satisfied: enrich>=1.2.6 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (1.2.7)
Requirement already satisfied: rich>=9.5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (12.6.0)
Requirement already satisfied: packaging in /usr/local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (21.3)
Requirement already satisfied: ruamel.yaml<1,>=0.15.34 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (0.18.3)
Requirement already satisfied: typing-extensions in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (4.1.1)
Requirement already satisfied: pathspec>=0.5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from yamllint==1.26.3->-r tox-requirements.txt (line 3)) (0.9.0)
Requirement already satisfied: setuptools in /usr/local/lib/python3.6/site-packages (from yamllint==1.26.3->-r tox-requirements.txt (line 3)) (59.6.0)
Requirement already satisfied: click<9,>=8.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (8.0.4)
Requirement already satisfied: click-help-colors>=0.9 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.9.4)
Requirement already satisfied: subprocess-tee>=0.3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.3.5)
Requirement already satisfied: pluggy<1.0,>=0.7.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.13.1)
Requirement already satisfied: cerberus!=1.3.3,!=1.3.4,>=1.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.3.5)
Requirement already satisfied: paramiko<3,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (2.12.0)
Requirement already satisfied: cookiecutter>=1.7.3 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.7.3)
Requirement already satisfied: Jinja2>=2.11.3 in /usr/local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (3.0.3)
Requirement already satisfied: dataclasses in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.8)
Requirement already satisfied: distro>=1.3.0 in /usr/local/lib/python3.6/site-packages (from selinux->-r tox-requirements.txt (line 1)) (1.9.0)
Requirement already satisfied: requests in /home/jenkins/.local/lib/python3.6/site-packages (from molecule_docker->-r tox-requirements.txt (line 6)) (2.27.1)
Requirement already satisfied: docker>=4.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule_docker->-r tox-requirements.txt (line 6)) (5.0.3)
Requirement already satisfied: ansible-compat>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule_docker->-r tox-requirements.txt (line 6)) (1.0.0)
Requirement already satisfied: cached-property~=1.5 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-compat>=0.5.0->molecule_docker->-r tox-requirements.txt (line 6)) (1.5.2)
Requirement already satisfied: importlib-metadata in /home/jenkins/.local/lib/python3.6/site-packages (from cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule==3.4.0->-r tox-requirements.txt (line 5)) (4.2.0)
Requirement already satisfied: binaryornot>=0.4.4 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.4.4)
Requirement already satisfied: six>=1.10 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.16.0)
Requirement already satisfied: python-slugify>=4.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (6.1.2)
Requirement already satisfied: jinja2-time>=0.2.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.2.0)
Requirement already satisfied: poyo>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.5.0)
Requirement already satisfied: websocket-client>=0.32.0 in /home/jenkins/.local/lib/python3.6/site-packages (from docker>=4.3.1->molecule_docker->-r tox-requirements.txt (line 6)) (1.3.1)
Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib64/python3.6/site-packages (from Jinja2>=2.11.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (2.0.1)
Requirement already satisfied: pynacl>=1.0.1 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.5.0)
Requirement already satisfied: bcrypt>=3.1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (4.0.1)
Requirement already satisfied: cryptography>=2.5 in /usr/local/lib64/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (40.0.2)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jenkins/.local/lib/python3.6/site-packages (from requests->molecule_docker->-r tox-requirements.txt (line 6)) (1.26.20)
Requirement already satisfied: certifi>=2017.4.17 in /home/jenkins/.local/lib/python3.6/site-packages (from requests->molecule_docker->-r tox-requirements.txt (line 6)) (2024.8.30)
Requirement already satisfied: charset-normalizer~=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from requests->molecule_docker->-r tox-requirements.txt (line 6)) (2.0.12)
Requirement already satisfied: idna<4,>=2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from requests->molecule_docker->-r tox-requirements.txt (line 6)) (3.10)
Requirement already satisfied: pygments<3.0.0,>=2.6.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (2.14.0)
Requirement already satisfied: commonmark<0.10.0,>=0.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (0.9.1)
Requirement already satisfied: ruamel.yaml.clib>=0.2.7 in /home/jenkins/.local/lib/python3.6/site-packages (from ruamel.yaml<1,>=0.15.34->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (0.2.8)
Requirement already satisfied: bracex>=2.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from wcmatch>=7.0->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (2.2.1)
Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /usr/local/lib/python3.6/site-packages (from packaging->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (3.1.4)
Requirement already satisfied: chardet>=3.0.2 in /home/jenkins/.local/lib/python3.6/site-packages (from binaryornot>=0.4.4->cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (5.0.0)
Requirement already satisfied: cffi>=1.12 in /usr/local/lib64/python3.6/site-packages (from cryptography>=2.5->paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.15.1)
Requirement already satisfied: zipp>=0.5 in /home/jenkins/.local/lib/python3.6/site-packages (from importlib-metadata->cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule==3.4.0->-r tox-requirements.txt (line 5)) (3.6.0)
Requirement already satisfied: arrow in /home/jenkins/.local/lib/python3.6/site-packages (from jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.2.3)
Requirement already satisfied: text-unidecode>=1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from python-slugify>=4.0.0->cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.3)
Requirement already satisfied: pycparser in /usr/local/lib/python3.6/site-packages (from cffi>=1.12->cryptography>=2.5->paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (2.21)
Requirement already satisfied: python-dateutil>=2.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from arrow->jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (2.9.0.post0)
+ pip install 'molecule[lint]'
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: molecule[lint] in /home/jenkins/.local/lib/python3.6/site-packages (3.4.0)
Requirement already satisfied: pluggy<1.0,>=0.7.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (0.13.1)
Requirement already satisfied: Jinja2>=2.11.3 in /usr/local/lib/python3.6/site-packages (from molecule[lint]) (3.0.3)
Requirement already satisfied: cookiecutter>=1.7.3 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (1.7.3)
Requirement already satisfied: rich>=9.5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (12.6.0)
Requirement already satisfied: ansible-lint>=5.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (5.1.3)
Requirement already satisfied: paramiko<3,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (2.12.0)
Requirement already satisfied: dataclasses in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (0.8)
Requirement already satisfied: enrich>=1.2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (1.2.7)
Requirement already satisfied: subprocess-tee>=0.3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (0.3.5)
Requirement already satisfied: selinux in /usr/local/lib/python3.6/site-packages (from molecule[lint]) (0.2.1)
Requirement already satisfied: PyYAML<6,>=5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (5.4.1)
Requirement already satisfied: setuptools>=42 in /usr/local/lib/python3.6/site-packages (from molecule[lint]) (59.6.0)
Requirement already satisfied: packaging in /usr/local/lib/python3.6/site-packages (from molecule[lint]) (21.3)
Requirement already satisfied: click-help-colors>=0.9 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (0.9.4)
Requirement already satisfied: click<9,>=8.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (8.0.4)
Requirement already satisfied: cerberus!=1.3.3,!=1.3.4,>=1.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (1.3.5)
Requirement already satisfied: flake8>=3.8.4 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (5.0.4)
Requirement already satisfied: pre-commit>=2.10.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (2.17.0)
Requirement already satisfied: yamllint in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (1.26.3)
Requirement already satisfied: ruamel.yaml<1,>=0.15.34 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[lint]) (0.18.3)
Requirement already satisfied: wcmatch>=7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[lint]) (8.3)
Requirement already satisfied: typing-extensions in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[lint]) (4.1.1)
Requirement already satisfied: tenacity in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[lint]) (8.2.2)
Requirement already satisfied: importlib-metadata in /home/jenkins/.local/lib/python3.6/site-packages (from cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule[lint]) (4.2.0)
Requirement already satisfied: poyo>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (0.5.0)
Requirement already satisfied: requests>=2.23.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (2.27.1)
Requirement already satisfied: jinja2-time>=0.2.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (0.2.0)
Requirement already satisfied: six>=1.10 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (1.16.0)
Requirement already satisfied: binaryornot>=0.4.4 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (0.4.4)
Requirement already satisfied: python-slugify>=4.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (6.1.2)
Requirement already satisfied: pyflakes<2.6.0,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[lint]) (2.5.0)
Requirement already satisfied: mccabe<0.8.0,>=0.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[lint]) (0.7.0)
Requirement already satisfied: pycodestyle<2.10.0,>=2.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[lint]) (2.9.1)
Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib64/python3.6/site-packages (from Jinja2>=2.11.3->molecule[lint]) (2.0.1)
Requirement already satisfied: bcrypt>=3.1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[lint]) (4.0.1)
Requirement already satisfied: cryptography>=2.5 in /usr/local/lib64/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[lint]) (40.0.2)
Requirement already satisfied: pynacl>=1.0.1 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[lint]) (1.5.0)
Requirement already satisfied: toml in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (0.10.2)
Requirement already satisfied: identify>=1.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (2.4.4)
Requirement already satisfied: cfgv>=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (3.3.1)
Requirement already satisfied: nodeenv>=0.11.1 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (1.6.0)
Requirement already satisfied: virtualenv>=20.0.8 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (20.16.2)
Requirement already satisfied: importlib-resources<5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (5.2.3)
Requirement already satisfied: commonmark<0.10.0,>=0.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->molecule[lint]) (0.9.1)
Requirement already satisfied: pygments<3.0.0,>=2.6.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->molecule[lint]) (2.14.0)
Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /usr/local/lib/python3.6/site-packages (from packaging->molecule[lint]) (3.1.4)
Requirement already satisfied: distro>=1.3.0 in /usr/local/lib/python3.6/site-packages (from selinux->molecule[lint]) (1.9.0)
Requirement already satisfied: pathspec>=0.5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from yamllint->molecule[lint]) (0.9.0)
Requirement already satisfied: chardet>=3.0.2 in /home/jenkins/.local/lib/python3.6/site-packages (from binaryornot>=0.4.4->cookiecutter>=1.7.3->molecule[lint]) (5.0.0)
Requirement already satisfied: cffi>=1.12 in /usr/local/lib64/python3.6/site-packages (from cryptography>=2.5->paramiko<3,>=2.5.0->molecule[lint]) (1.15.1)
Requirement already satisfied: zipp>=0.5 in /home/jenkins/.local/lib/python3.6/site-packages (from importlib-metadata->cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule[lint]) (3.6.0)
Requirement already satisfied: arrow in /home/jenkins/.local/lib/python3.6/site-packages (from jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule[lint]) (1.2.3)
Requirement already satisfied: text-unidecode>=1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from python-slugify>=4.0.0->cookiecutter>=1.7.3->molecule[lint]) (1.3)
Requirement already satisfied: certifi>=2017.4.17 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[lint]) (2024.8.30)
Requirement already satisfied: idna<4,>=2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[lint]) (3.10)
Requirement already satisfied: charset-normalizer~=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[lint]) (2.0.12)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[lint]) (1.26.20)
Requirement already satisfied: ruamel.yaml.clib>=0.2.7 in /home/jenkins/.local/lib/python3.6/site-packages (from ruamel.yaml<1,>=0.15.34->ansible-lint>=5.1.1->molecule[lint]) (0.2.8)
Requirement already satisfied: distlib<1,>=0.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[lint]) (0.3.8)
Requirement already satisfied: platformdirs<3,>=2 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[lint]) (2.4.0)
Requirement already satisfied: filelock<4,>=3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[lint]) (3.4.1)
Requirement already satisfied: bracex>=2.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from wcmatch>=7.0->ansible-lint>=5.1.1->molecule[lint]) (2.2.1)
Requirement already satisfied: pycparser in /usr/local/lib/python3.6/site-packages (from cffi>=1.12->cryptography>=2.5->paramiko<3,>=2.5.0->molecule[lint]) (2.21)
Requirement already satisfied: python-dateutil>=2.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from arrow->jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule[lint]) (2.9.0.post0)
+ pip install 'molecule[docker,lint]'
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: molecule[docker,lint] in /home/jenkins/.local/lib/python3.6/site-packages (3.4.0)
Requirement already satisfied: Jinja2>=2.11.3 in /usr/local/lib/python3.6/site-packages (from molecule[docker,lint]) (3.0.3)
Requirement already satisfied: rich>=9.5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (12.6.0)
Requirement already satisfied: setuptools>=42 in /usr/local/lib/python3.6/site-packages (from molecule[docker,lint]) (59.6.0)
Requirement already satisfied: enrich>=1.2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.2.7)
Requirement already satisfied: selinux in /usr/local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.2.1)
Requirement already satisfied: click<9,>=8.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (8.0.4)
Requirement already satisfied: click-help-colors>=0.9 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.9.4)
Requirement already satisfied: subprocess-tee>=0.3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.3.5)
Requirement already satisfied: cerberus!=1.3.3,!=1.3.4,>=1.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.3.5)
Requirement already satisfied: packaging in /usr/local/lib/python3.6/site-packages (from molecule[docker,lint]) (21.3)
Requirement already satisfied: pluggy<1.0,>=0.7.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.13.1)
Requirement already satisfied: dataclasses in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.8)
Requirement already satisfied: ansible-lint>=5.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (5.1.3)
Requirement already satisfied: cookiecutter>=1.7.3 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.7.3)
Requirement already satisfied: PyYAML<6,>=5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (5.4.1)
Requirement already satisfied: paramiko<3,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (2.12.0)
Requirement already satisfied: pre-commit>=2.10.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (2.17.0)
Requirement already satisfied: yamllint in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.26.3)
Requirement already satisfied: flake8>=3.8.4 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (5.0.4)
Requirement already satisfied: molecule-docker in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.1.0)
Requirement already satisfied: typing-extensions in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[docker,lint]) (4.1.1)
Requirement already satisfied: wcmatch>=7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[docker,lint]) (8.3)
Requirement already satisfied: tenacity in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[docker,lint]) (8.2.2)
Requirement already satisfied: ruamel.yaml<1,>=0.15.34 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[docker,lint]) (0.18.3)
Requirement already satisfied: importlib-metadata in /home/jenkins/.local/lib/python3.6/site-packages (from cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule[docker,lint]) (4.2.0)
Requirement already satisfied: poyo>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (0.5.0)
Requirement already satisfied: binaryornot>=0.4.4 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (0.4.4)
Requirement already satisfied: python-slugify>=4.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (6.1.2)
Requirement already satisfied: six>=1.10 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (1.16.0)
Requirement already satisfied: requests>=2.23.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (2.27.1)
Requirement already satisfied: jinja2-time>=0.2.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (0.2.0)
Requirement already satisfied: mccabe<0.8.0,>=0.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[docker,lint]) (0.7.0)
Requirement already satisfied: pyflakes<2.6.0,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[docker,lint]) (2.5.0)
Requirement already satisfied: pycodestyle<2.10.0,>=2.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[docker,lint]) (2.9.1)
Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib64/python3.6/site-packages (from Jinja2>=2.11.3->molecule[docker,lint]) (2.0.1)
Requirement already satisfied: cryptography>=2.5 in /usr/local/lib64/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[docker,lint]) (40.0.2)
Requirement already satisfied: pynacl>=1.0.1 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[docker,lint]) (1.5.0)
Requirement already satisfied: bcrypt>=3.1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[docker,lint]) (4.0.1)
Requirement already satisfied: nodeenv>=0.11.1 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (1.6.0)
Requirement already satisfied: identify>=1.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (2.4.4)
Requirement already satisfied: cfgv>=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (3.3.1)
Requirement already satisfied: virtualenv>=20.0.8 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (20.16.2)
Requirement already satisfied: toml in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (0.10.2)
Requirement already satisfied: importlib-resources<5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (5.2.3)
Requirement already satisfied: pygments<3.0.0,>=2.6.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->molecule[docker,lint]) (2.14.0)
Requirement already satisfied: commonmark<0.10.0,>=0.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->molecule[docker,lint]) (0.9.1)
Requirement already satisfied: ansible-compat>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule-docker->molecule[docker,lint]) (1.0.0)
Requirement already satisfied: docker>=4.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule-docker->molecule[docker,lint]) (5.0.3)
Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /usr/local/lib/python3.6/site-packages (from packaging->molecule[docker,lint]) (3.1.4)
Requirement already satisfied: distro>=1.3.0 in /usr/local/lib/python3.6/site-packages (from selinux->molecule[docker,lint]) (1.9.0)
Requirement already satisfied: pathspec>=0.5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from yamllint->molecule[docker,lint]) (0.9.0)
Requirement already satisfied: cached-property~=1.5 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-compat>=0.5.0->molecule-docker->molecule[docker,lint]) (1.5.2)
Requirement already satisfied: chardet>=3.0.2 in /home/jenkins/.local/lib/python3.6/site-packages (from binaryornot>=0.4.4->cookiecutter>=1.7.3->molecule[docker,lint]) (5.0.0)
Requirement already satisfied: cffi>=1.12 in /usr/local/lib64/python3.6/site-packages (from cryptography>=2.5->paramiko<3,>=2.5.0->molecule[docker,lint]) (1.15.1)
Requirement already satisfied: websocket-client>=0.32.0 in /home/jenkins/.local/lib/python3.6/site-packages (from docker>=4.3.1->molecule-docker->molecule[docker,lint]) (1.3.1)
Requirement already satisfied: zipp>=0.5 in /home/jenkins/.local/lib/python3.6/site-packages (from importlib-metadata->cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule[docker,lint]) (3.6.0)
Requirement already satisfied: arrow in /home/jenkins/.local/lib/python3.6/site-packages (from jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule[docker,lint]) (1.2.3)
Requirement already satisfied: text-unidecode>=1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from python-slugify>=4.0.0->cookiecutter>=1.7.3->molecule[docker,lint]) (1.3)
Requirement already satisfied: idna<4,>=2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[docker,lint]) (3.10)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[docker,lint]) (1.26.20)
Requirement already satisfied: certifi>=2017.4.17 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[docker,lint]) (2024.8.30)
Requirement already satisfied: charset-normalizer~=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[docker,lint]) (2.0.12)
Requirement already satisfied: ruamel.yaml.clib>=0.2.7 in /home/jenkins/.local/lib/python3.6/site-packages (from ruamel.yaml<1,>=0.15.34->ansible-lint>=5.1.1->molecule[docker,lint]) (0.2.8)
Requirement already satisfied: distlib<1,>=0.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[docker,lint]) (0.3.8)
Requirement already satisfied: platformdirs<3,>=2 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[docker,lint]) (2.4.0)
Requirement already satisfied: filelock<4,>=3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[docker,lint]) (3.4.1)
Requirement already satisfied: bracex>=2.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from wcmatch>=7.0->ansible-lint>=5.1.1->molecule[docker,lint]) (2.2.1)
Requirement already satisfied: pycparser in /usr/local/lib/python3.6/site-packages (from cffi>=1.12->cryptography>=2.5->paramiko<3,>=2.5.0->molecule[docker,lint]) (2.21)
Requirement already satisfied: python-dateutil>=2.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from arrow->jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule[docker,lint]) (2.9.0.post0)
+ molecule test
/home/jenkins/.local/lib/python3.6/site-packages/requests/__init__.py:104: RequestsDependencyWarning: urllib3 (1.26.20) or chardet (5.0.0)/charset_normalizer (2.0.12) doesn't match a supported version!
  RequestsDependencyWarning)
INFO     default scenario test matrix: create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
INFO     Guessed /opt/jenkins_agent/workspace/freestyle as project root directory
INFO     Using /home/jenkins/.cache/ansible-lint/e90229/roles/georgy.vector symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/home/jenkins/.cache/ansible-lint/e90229/roles
INFO     Running default > create
INFO     Sanity checks: 'docker'
/home/jenkins/.local/lib/python3.6/site-packages/paramiko/transport.py:33: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.hazmat.backends import default_backend

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 2, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/quay.io/centos/centos:stream8)
skipping: [localhost] => (item=molecule_local/pycontribs/ubuntu:latest)
skipping: [localhost] => (item=molecule_local/pycontribs/centos:7)

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance)
changed: [localhost] => (item=ubuntu)
changed: [localhost] => (item=centos)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '671629793554.4327', 'results_file': '/home/jenkins/.ansible_async/671629793554.4327', 'changed': True, 'failed': False, 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '878001555663.4356', 'results_file': '/home/jenkins/.ansible_async/878001555663.4356', 'changed': True, 'failed': False, 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '434087967639.4405', 'results_file': '/home/jenkins/.ansible_async/434087967639.4405', 'changed': True, 'failed': False, 'item': {'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
ok: [instance]
ok: [ubuntu]
ok: [centos]

TASK [Include vector] **********************************************************

TASK [georgy.vector : Create directory vector] *********************************
changed: [instance]
changed: [ubuntu]
changed: [centos]

TASK [georgy.vector : Get vector distrib] **************************************
changed: [centos]
changed: [instance]
changed: [ubuntu]

TASK [georgy.vector : Unarchive vector] ****************************************
changed: [ubuntu]
changed: [instance]
changed: [centos]

TASK [georgy.vector : Create a symbolic link] **********************************
changed: [centos]
changed: [ubuntu]
changed: [instance]

TASK [georgy.vector : Create vector unit flie] *********************************
changed: [ubuntu]
changed: [instance]
changed: [centos]

TASK [georgy.vector : Mkdir vector data] ***************************************
changed: [centos]
changed: [instance]
changed: [ubuntu]

TASK [georgy.vector : Vector config create] ************************************
changed: [centos]
changed: [ubuntu]
changed: [instance]

PLAY RECAP *********************************************************************
centos                     : ok=8    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
instance                   : ok=8    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=8    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
ok: [ubuntu]
ok: [instance]
ok: [centos]

TASK [Include vector] **********************************************************

TASK [georgy.vector : Create directory vector] *********************************
ok: [instance]
ok: [centos]
ok: [ubuntu]

TASK [georgy.vector : Get vector distrib] **************************************
ok: [centos]
ok: [ubuntu]
ok: [instance]

TASK [georgy.vector : Unarchive vector] ****************************************
skipping: [instance]
skipping: [centos]
skipping: [ubuntu]

TASK [georgy.vector : Create a symbolic link] **********************************
ok: [centos]
ok: [instance]
ok: [ubuntu]

TASK [georgy.vector : Create vector unit flie] *********************************
ok: [centos]
ok: [instance]
ok: [ubuntu]

TASK [georgy.vector : Mkdir vector data] ***************************************
ok: [centos]
ok: [instance]
ok: [ubuntu]

TASK [georgy.vector : Vector config create] ************************************
ok: [centos]
ok: [instance]
ok: [ubuntu]

PLAY RECAP *********************************************************************
centos                     : ok=7    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
instance                   : ok=7    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ubuntu                     : ok=7    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Gathering Facts] *********************************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
ok: [ubuntu]
ok: [instance]
ok: [centos]

TASK [Get Vector version] ******************************************************
ok: [ubuntu]
ok: [centos]
ok: [instance]

TASK [Assert Vector instalation] ***********************************************
ok: [centos] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [instance] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

TASK [Validation Vector configuration] *****************************************
ok: [instance]
ok: [centos]
ok: [ubuntu]

TASK [Assert Vector validate config] *******************************************
ok: [centos] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [instance] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos                     : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
instance                   : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
changed: [localhost] => (item=instance)
changed: [localhost] => (item=ubuntu)
changed: [localhost] => (item=centos)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=instance)
changed: [localhost] => (item=ubuntu)
changed: [localhost] => (item=centos)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
Finished: SUCCESS
```

3. Перенести Declarative Pipeline в репозиторий в файл `Jenkinsfile`.

```
pipeline {
    agent any
    stages {
        stage('GIT checkout') {
            steps {
                echo 'Get from GIT repository'
                git credentialsId: 'stepynin-georgy', 
                url: 'https://github.com/stepynin-georgy/vector-role.git',
                branch: 'tests'
            }
        }
        stage('preparation') {
            steps {
                echo 'Start preparation'
                sh 'pip3 install -r tox-requirements.txt'
                sh 'pip install "molecule[lint]"'
                sh 'pip install "molecule[docker,lint]"'
            }
        }
        stage('Start molecule test') {
            steps {
                echo 'Run molecule test'
                sh 'molecule test'
            }
        }
    }
}
```

```
Started by user admin
Obtained Jenkinsfile from git https://github.com/stepynin-georgy/vector-role.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on agent in /opt/jenkins_agent/workspace/declarative pipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential e04d7e0d-faee-40f4-9a9c-c6ea83fbb998
Cloning the remote Git repository
Cloning repository https://github.com/stepynin-georgy/vector-role.git
 > git init /opt/jenkins_agent/workspace/declarative pipeline # timeout=10
Fetching upstream changes from https://github.com/stepynin-georgy/vector-role.git
 > git --version # timeout=10
 > git --version # 'git version 1.8.3.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --progress https://github.com/stepynin-georgy/vector-role.git +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
Checking out Revision a9bf3313b725579e6860628844d5fead8698b1de (refs/remotes/origin/tests)
Commit message: "Update Jenkinsfile"
First time build. Skipping changelog.
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (GIT checkout)
[Pipeline] echo
Get from GIT repository
[Pipeline] git
 > git config remote.origin.url https://github.com/stepynin-georgy/vector-role.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/tests^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f a9bf3313b725579e6860628844d5fead8698b1de # timeout=10
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
Warning: CredentialId "stepynin-georgy" could not be found.
Fetching changes from the remote Git repository
Checking out Revision a9bf3313b725579e6860628844d5fead8698b1de (refs/remotes/origin/tests)
Commit message: "Update Jenkinsfile"
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (preparation)
[Pipeline] echo
Start preparation
 > git rev-parse --resolve-git-dir /opt/jenkins_agent/workspace/declarative pipeline/.git # timeout=10
 > git config remote.origin.url https://github.com/stepynin-georgy/vector-role.git # timeout=10
Fetching upstream changes from https://github.com/stepynin-georgy/vector-role.git
 > git --version # timeout=10
 > git --version # 'git version 1.8.3.1'
 > git fetch --tags --progress https://github.com/stepynin-georgy/vector-role.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/tests^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f a9bf3313b725579e6860628844d5fead8698b1de # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git checkout -b tests a9bf3313b725579e6860628844d5fead8698b1de # timeout=10
[Pipeline] sh
+ pip3 install -r tox-requirements.txt
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: selinux in /usr/local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 1)) (0.2.1)
Requirement already satisfied: ansible-lint==5.1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 2)) (5.1.3)
Requirement already satisfied: yamllint==1.26.3 in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 3)) (1.26.3)
Requirement already satisfied: lxml in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 4)) (5.3.0)
Requirement already satisfied: molecule==3.4.0 in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 5)) (3.4.0)
Requirement already satisfied: molecule_docker in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 6)) (1.1.0)
Requirement already satisfied: jmespath in /home/jenkins/.local/lib/python3.6/site-packages (from -r tox-requirements.txt (line 7)) (0.10.0)
Requirement already satisfied: enrich>=1.2.6 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (1.2.7)
Requirement already satisfied: ruamel.yaml<1,>=0.15.34 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (0.18.3)
Requirement already satisfied: packaging in /usr/local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (21.3)
Requirement already satisfied: tenacity in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (8.2.2)
Requirement already satisfied: wcmatch>=7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (8.3)
Requirement already satisfied: pyyaml in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (5.4.1)
Requirement already satisfied: rich>=9.5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (12.6.0)
Requirement already satisfied: typing-extensions in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (4.1.1)
Requirement already satisfied: pathspec>=0.5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from yamllint==1.26.3->-r tox-requirements.txt (line 3)) (0.9.0)
Requirement already satisfied: setuptools in /usr/local/lib/python3.6/site-packages (from yamllint==1.26.3->-r tox-requirements.txt (line 3)) (59.6.0)
Requirement already satisfied: click-help-colors>=0.9 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.9.4)
Requirement already satisfied: dataclasses in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.8)
Requirement already satisfied: pluggy<1.0,>=0.7.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.13.1)
Requirement already satisfied: click<9,>=8.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (8.0.4)
Requirement already satisfied: Jinja2>=2.11.3 in /usr/local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (3.0.3)
Requirement already satisfied: paramiko<3,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (2.12.0)
Requirement already satisfied: subprocess-tee>=0.3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.3.5)
Requirement already satisfied: cerberus!=1.3.3,!=1.3.4,>=1.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.3.5)
Requirement already satisfied: cookiecutter>=1.7.3 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.7.3)
Requirement already satisfied: distro>=1.3.0 in /usr/local/lib/python3.6/site-packages (from selinux->-r tox-requirements.txt (line 1)) (1.9.0)
Requirement already satisfied: ansible-compat>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule_docker->-r tox-requirements.txt (line 6)) (1.0.0)
Requirement already satisfied: docker>=4.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule_docker->-r tox-requirements.txt (line 6)) (5.0.3)
Requirement already satisfied: requests in /home/jenkins/.local/lib/python3.6/site-packages (from molecule_docker->-r tox-requirements.txt (line 6)) (2.27.1)
Requirement already satisfied: cached-property~=1.5 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-compat>=0.5.0->molecule_docker->-r tox-requirements.txt (line 6)) (1.5.2)
Requirement already satisfied: importlib-metadata in /home/jenkins/.local/lib/python3.6/site-packages (from cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule==3.4.0->-r tox-requirements.txt (line 5)) (4.2.0)
Requirement already satisfied: poyo>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.5.0)
Requirement already satisfied: python-slugify>=4.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (6.1.2)
Requirement already satisfied: binaryornot>=0.4.4 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.4.4)
Requirement already satisfied: six>=1.10 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.16.0)
Requirement already satisfied: jinja2-time>=0.2.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (0.2.0)
Requirement already satisfied: websocket-client>=0.32.0 in /home/jenkins/.local/lib/python3.6/site-packages (from docker>=4.3.1->molecule_docker->-r tox-requirements.txt (line 6)) (1.3.1)
Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib64/python3.6/site-packages (from Jinja2>=2.11.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (2.0.1)
Requirement already satisfied: bcrypt>=3.1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (4.0.1)
Requirement already satisfied: cryptography>=2.5 in /usr/local/lib64/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (40.0.2)
Requirement already satisfied: pynacl>=1.0.1 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.5.0)
Requirement already satisfied: certifi>=2017.4.17 in /home/jenkins/.local/lib/python3.6/site-packages (from requests->molecule_docker->-r tox-requirements.txt (line 6)) (2024.8.30)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jenkins/.local/lib/python3.6/site-packages (from requests->molecule_docker->-r tox-requirements.txt (line 6)) (1.26.20)
Requirement already satisfied: idna<4,>=2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from requests->molecule_docker->-r tox-requirements.txt (line 6)) (3.10)
Requirement already satisfied: charset-normalizer~=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from requests->molecule_docker->-r tox-requirements.txt (line 6)) (2.0.12)
Requirement already satisfied: pygments<3.0.0,>=2.6.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (2.14.0)
Requirement already satisfied: commonmark<0.10.0,>=0.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (0.9.1)
Requirement already satisfied: ruamel.yaml.clib>=0.2.7 in /home/jenkins/.local/lib/python3.6/site-packages (from ruamel.yaml<1,>=0.15.34->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (0.2.8)
Requirement already satisfied: bracex>=2.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from wcmatch>=7.0->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (2.2.1)
Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /usr/local/lib/python3.6/site-packages (from packaging->ansible-lint==5.1.3->-r tox-requirements.txt (line 2)) (3.1.4)
Requirement already satisfied: chardet>=3.0.2 in /home/jenkins/.local/lib/python3.6/site-packages (from binaryornot>=0.4.4->cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (5.0.0)
Requirement already satisfied: cffi>=1.12 in /usr/local/lib64/python3.6/site-packages (from cryptography>=2.5->paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.15.1)
Requirement already satisfied: zipp>=0.5 in /home/jenkins/.local/lib/python3.6/site-packages (from importlib-metadata->cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule==3.4.0->-r tox-requirements.txt (line 5)) (3.6.0)
Requirement already satisfied: arrow in /home/jenkins/.local/lib/python3.6/site-packages (from jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.2.3)
Requirement already satisfied: text-unidecode>=1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from python-slugify>=4.0.0->cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (1.3)
Requirement already satisfied: pycparser in /usr/local/lib/python3.6/site-packages (from cffi>=1.12->cryptography>=2.5->paramiko<3,>=2.5.0->molecule==3.4.0->-r tox-requirements.txt (line 5)) (2.21)
Requirement already satisfied: python-dateutil>=2.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from arrow->jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule==3.4.0->-r tox-requirements.txt (line 5)) (2.9.0.post0)
[Pipeline] sh
+ pip install 'molecule[lint]'
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: molecule[lint] in /home/jenkins/.local/lib/python3.6/site-packages (3.4.0)
Requirement already satisfied: selinux in /usr/local/lib/python3.6/site-packages (from molecule[lint]) (0.2.1)
Requirement already satisfied: click<9,>=8.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (8.0.4)
Requirement already satisfied: packaging in /usr/local/lib/python3.6/site-packages (from molecule[lint]) (21.3)
Requirement already satisfied: ansible-lint>=5.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (5.1.3)
Requirement already satisfied: dataclasses in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (0.8)
Requirement already satisfied: cookiecutter>=1.7.3 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (1.7.3)
Requirement already satisfied: cerberus!=1.3.3,!=1.3.4,>=1.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (1.3.5)
Requirement already satisfied: Jinja2>=2.11.3 in /usr/local/lib/python3.6/site-packages (from molecule[lint]) (3.0.3)
Requirement already satisfied: setuptools>=42 in /usr/local/lib/python3.6/site-packages (from molecule[lint]) (59.6.0)
Requirement already satisfied: subprocess-tee>=0.3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (0.3.5)
Requirement already satisfied: paramiko<3,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (2.12.0)
Requirement already satisfied: pluggy<1.0,>=0.7.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (0.13.1)
Requirement already satisfied: rich>=9.5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (12.6.0)
Requirement already satisfied: click-help-colors>=0.9 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (0.9.4)
Requirement already satisfied: PyYAML<6,>=5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (5.4.1)
Requirement already satisfied: enrich>=1.2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (1.2.7)
Requirement already satisfied: pre-commit>=2.10.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (2.17.0)
Requirement already satisfied: flake8>=3.8.4 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (5.0.4)
Requirement already satisfied: yamllint in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[lint]) (1.26.3)
Requirement already satisfied: wcmatch>=7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[lint]) (8.3)
Requirement already satisfied: tenacity in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[lint]) (8.2.2)
Requirement already satisfied: ruamel.yaml<1,>=0.15.34 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[lint]) (0.18.3)
Requirement already satisfied: typing-extensions in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[lint]) (4.1.1)
Requirement already satisfied: importlib-metadata in /home/jenkins/.local/lib/python3.6/site-packages (from cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule[lint]) (4.2.0)
Requirement already satisfied: six>=1.10 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (1.16.0)
Requirement already satisfied: jinja2-time>=0.2.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (0.2.0)
Requirement already satisfied: python-slugify>=4.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (6.1.2)
Requirement already satisfied: requests>=2.23.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (2.27.1)
Requirement already satisfied: binaryornot>=0.4.4 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (0.4.4)
Requirement already satisfied: poyo>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[lint]) (0.5.0)
Requirement already satisfied: pycodestyle<2.10.0,>=2.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[lint]) (2.9.1)
Requirement already satisfied: pyflakes<2.6.0,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[lint]) (2.5.0)
Requirement already satisfied: mccabe<0.8.0,>=0.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[lint]) (0.7.0)
Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib64/python3.6/site-packages (from Jinja2>=2.11.3->molecule[lint]) (2.0.1)
Requirement already satisfied: cryptography>=2.5 in /usr/local/lib64/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[lint]) (40.0.2)
Requirement already satisfied: pynacl>=1.0.1 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[lint]) (1.5.0)
Requirement already satisfied: bcrypt>=3.1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[lint]) (4.0.1)
Requirement already satisfied: nodeenv>=0.11.1 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (1.6.0)
Requirement already satisfied: identify>=1.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (2.4.4)
Requirement already satisfied: virtualenv>=20.0.8 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (20.16.2)
Requirement already satisfied: cfgv>=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (3.3.1)
Requirement already satisfied: importlib-resources<5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (5.2.3)
Requirement already satisfied: toml in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[lint]) (0.10.2)
Requirement already satisfied: commonmark<0.10.0,>=0.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->molecule[lint]) (0.9.1)
Requirement already satisfied: pygments<3.0.0,>=2.6.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->molecule[lint]) (2.14.0)
Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /usr/local/lib/python3.6/site-packages (from packaging->molecule[lint]) (3.1.4)
Requirement already satisfied: distro>=1.3.0 in /usr/local/lib/python3.6/site-packages (from selinux->molecule[lint]) (1.9.0)
Requirement already satisfied: pathspec>=0.5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from yamllint->molecule[lint]) (0.9.0)
Requirement already satisfied: chardet>=3.0.2 in /home/jenkins/.local/lib/python3.6/site-packages (from binaryornot>=0.4.4->cookiecutter>=1.7.3->molecule[lint]) (5.0.0)
Requirement already satisfied: cffi>=1.12 in /usr/local/lib64/python3.6/site-packages (from cryptography>=2.5->paramiko<3,>=2.5.0->molecule[lint]) (1.15.1)
Requirement already satisfied: zipp>=0.5 in /home/jenkins/.local/lib/python3.6/site-packages (from importlib-metadata->cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule[lint]) (3.6.0)
Requirement already satisfied: arrow in /home/jenkins/.local/lib/python3.6/site-packages (from jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule[lint]) (1.2.3)
Requirement already satisfied: text-unidecode>=1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from python-slugify>=4.0.0->cookiecutter>=1.7.3->molecule[lint]) (1.3)
Requirement already satisfied: certifi>=2017.4.17 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[lint]) (2024.8.30)
Requirement already satisfied: idna<4,>=2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[lint]) (3.10)
Requirement already satisfied: charset-normalizer~=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[lint]) (2.0.12)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[lint]) (1.26.20)
Requirement already satisfied: ruamel.yaml.clib>=0.2.7 in /home/jenkins/.local/lib/python3.6/site-packages (from ruamel.yaml<1,>=0.15.34->ansible-lint>=5.1.1->molecule[lint]) (0.2.8)
Requirement already satisfied: platformdirs<3,>=2 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[lint]) (2.4.0)
Requirement already satisfied: filelock<4,>=3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[lint]) (3.4.1)
Requirement already satisfied: distlib<1,>=0.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[lint]) (0.3.8)
Requirement already satisfied: bracex>=2.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from wcmatch>=7.0->ansible-lint>=5.1.1->molecule[lint]) (2.2.1)
Requirement already satisfied: pycparser in /usr/local/lib/python3.6/site-packages (from cffi>=1.12->cryptography>=2.5->paramiko<3,>=2.5.0->molecule[lint]) (2.21)
Requirement already satisfied: python-dateutil>=2.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from arrow->jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule[lint]) (2.9.0.post0)
[Pipeline] sh
+ pip install 'molecule[docker,lint]'
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: molecule[docker,lint] in /home/jenkins/.local/lib/python3.6/site-packages (3.4.0)
Requirement already satisfied: dataclasses in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.8)
Requirement already satisfied: rich>=9.5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (12.6.0)
Requirement already satisfied: PyYAML<6,>=5.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (5.4.1)
Requirement already satisfied: paramiko<3,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (2.12.0)
Requirement already satisfied: setuptools>=42 in /usr/local/lib/python3.6/site-packages (from molecule[docker,lint]) (59.6.0)
Requirement already satisfied: Jinja2>=2.11.3 in /usr/local/lib/python3.6/site-packages (from molecule[docker,lint]) (3.0.3)
Requirement already satisfied: click-help-colors>=0.9 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.9.4)
Requirement already satisfied: cerberus!=1.3.3,!=1.3.4,>=1.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.3.5)
Requirement already satisfied: selinux in /usr/local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.2.1)
Requirement already satisfied: subprocess-tee>=0.3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.3.5)
Requirement already satisfied: pluggy<1.0,>=0.7.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (0.13.1)
Requirement already satisfied: packaging in /usr/local/lib/python3.6/site-packages (from molecule[docker,lint]) (21.3)
Requirement already satisfied: enrich>=1.2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.2.7)
Requirement already satisfied: cookiecutter>=1.7.3 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.7.3)
Requirement already satisfied: ansible-lint>=5.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (5.1.3)
Requirement already satisfied: click<9,>=8.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (8.0.4)
Requirement already satisfied: flake8>=3.8.4 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (5.0.4)
Requirement already satisfied: yamllint in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.26.3)
Requirement already satisfied: pre-commit>=2.10.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (2.17.0)
Requirement already satisfied: molecule-docker in /home/jenkins/.local/lib/python3.6/site-packages (from molecule[docker,lint]) (1.1.0)
Requirement already satisfied: ruamel.yaml<1,>=0.15.34 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[docker,lint]) (0.18.3)
Requirement already satisfied: wcmatch>=7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[docker,lint]) (8.3)
Requirement already satisfied: typing-extensions in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[docker,lint]) (4.1.1)
Requirement already satisfied: tenacity in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-lint>=5.1.1->molecule[docker,lint]) (8.2.2)
Requirement already satisfied: importlib-metadata in /home/jenkins/.local/lib/python3.6/site-packages (from cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule[docker,lint]) (4.2.0)
Requirement already satisfied: requests>=2.23.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (2.27.1)
Requirement already satisfied: python-slugify>=4.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (6.1.2)
Requirement already satisfied: jinja2-time>=0.2.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (0.2.0)
Requirement already satisfied: six>=1.10 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (1.16.0)
Requirement already satisfied: poyo>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (0.5.0)
Requirement already satisfied: binaryornot>=0.4.4 in /home/jenkins/.local/lib/python3.6/site-packages (from cookiecutter>=1.7.3->molecule[docker,lint]) (0.4.4)
Requirement already satisfied: mccabe<0.8.0,>=0.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[docker,lint]) (0.7.0)
Requirement already satisfied: pycodestyle<2.10.0,>=2.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[docker,lint]) (2.9.1)
Requirement already satisfied: pyflakes<2.6.0,>=2.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from flake8>=3.8.4->molecule[docker,lint]) (2.5.0)
Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib64/python3.6/site-packages (from Jinja2>=2.11.3->molecule[docker,lint]) (2.0.1)
Requirement already satisfied: pynacl>=1.0.1 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[docker,lint]) (1.5.0)
Requirement already satisfied: bcrypt>=3.1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[docker,lint]) (4.0.1)
Requirement already satisfied: cryptography>=2.5 in /usr/local/lib64/python3.6/site-packages (from paramiko<3,>=2.5.0->molecule[docker,lint]) (40.0.2)
Requirement already satisfied: identify>=1.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (2.4.4)
Requirement already satisfied: virtualenv>=20.0.8 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (20.16.2)
Requirement already satisfied: importlib-resources<5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (5.2.3)
Requirement already satisfied: toml in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (0.10.2)
Requirement already satisfied: nodeenv>=0.11.1 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (1.6.0)
Requirement already satisfied: cfgv>=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from pre-commit>=2.10.1->molecule[docker,lint]) (3.3.1)
Requirement already satisfied: pygments<3.0.0,>=2.6.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->molecule[docker,lint]) (2.14.0)
Requirement already satisfied: commonmark<0.10.0,>=0.9.0 in /home/jenkins/.local/lib/python3.6/site-packages (from rich>=9.5.1->molecule[docker,lint]) (0.9.1)
Requirement already satisfied: docker>=4.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule-docker->molecule[docker,lint]) (5.0.3)
Requirement already satisfied: ansible-compat>=0.5.0 in /home/jenkins/.local/lib/python3.6/site-packages (from molecule-docker->molecule[docker,lint]) (1.0.0)
Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /usr/local/lib/python3.6/site-packages (from packaging->molecule[docker,lint]) (3.1.4)
Requirement already satisfied: distro>=1.3.0 in /usr/local/lib/python3.6/site-packages (from selinux->molecule[docker,lint]) (1.9.0)
Requirement already satisfied: pathspec>=0.5.3 in /home/jenkins/.local/lib/python3.6/site-packages (from yamllint->molecule[docker,lint]) (0.9.0)
Requirement already satisfied: cached-property~=1.5 in /home/jenkins/.local/lib/python3.6/site-packages (from ansible-compat>=0.5.0->molecule-docker->molecule[docker,lint]) (1.5.2)
Requirement already satisfied: chardet>=3.0.2 in /home/jenkins/.local/lib/python3.6/site-packages (from binaryornot>=0.4.4->cookiecutter>=1.7.3->molecule[docker,lint]) (5.0.0)
Requirement already satisfied: cffi>=1.12 in /usr/local/lib64/python3.6/site-packages (from cryptography>=2.5->paramiko<3,>=2.5.0->molecule[docker,lint]) (1.15.1)
Requirement already satisfied: websocket-client>=0.32.0 in /home/jenkins/.local/lib/python3.6/site-packages (from docker>=4.3.1->molecule-docker->molecule[docker,lint]) (1.3.1)
Requirement already satisfied: zipp>=0.5 in /home/jenkins/.local/lib/python3.6/site-packages (from importlib-metadata->cerberus!=1.3.3,!=1.3.4,>=1.3.1->molecule[docker,lint]) (3.6.0)
Requirement already satisfied: arrow in /home/jenkins/.local/lib/python3.6/site-packages (from jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule[docker,lint]) (1.2.3)
Requirement already satisfied: text-unidecode>=1.3 in /home/jenkins/.local/lib/python3.6/site-packages (from python-slugify>=4.0.0->cookiecutter>=1.7.3->molecule[docker,lint]) (1.3)
Requirement already satisfied: certifi>=2017.4.17 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[docker,lint]) (2024.8.30)
Requirement already satisfied: charset-normalizer~=2.0.0 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[docker,lint]) (2.0.12)
Requirement already satisfied: idna<4,>=2.5 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[docker,lint]) (3.10)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jenkins/.local/lib/python3.6/site-packages (from requests>=2.23.0->cookiecutter>=1.7.3->molecule[docker,lint]) (1.26.20)
Requirement already satisfied: ruamel.yaml.clib>=0.2.7 in /home/jenkins/.local/lib/python3.6/site-packages (from ruamel.yaml<1,>=0.15.34->ansible-lint>=5.1.1->molecule[docker,lint]) (0.2.8)
Requirement already satisfied: filelock<4,>=3.2 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[docker,lint]) (3.4.1)
Requirement already satisfied: distlib<1,>=0.3.1 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[docker,lint]) (0.3.8)
Requirement already satisfied: platformdirs<3,>=2 in /home/jenkins/.local/lib/python3.6/site-packages (from virtualenv>=20.0.8->pre-commit>=2.10.1->molecule[docker,lint]) (2.4.0)
Requirement already satisfied: bracex>=2.1.1 in /home/jenkins/.local/lib/python3.6/site-packages (from wcmatch>=7.0->ansible-lint>=5.1.1->molecule[docker,lint]) (2.2.1)
Requirement already satisfied: pycparser in /usr/local/lib/python3.6/site-packages (from cffi>=1.12->cryptography>=2.5->paramiko<3,>=2.5.0->molecule[docker,lint]) (2.21)
Requirement already satisfied: python-dateutil>=2.7.0 in /home/jenkins/.local/lib/python3.6/site-packages (from arrow->jinja2-time>=0.2.0->cookiecutter>=1.7.3->molecule[docker,lint]) (2.9.0.post0)
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Start molecule test)
[Pipeline] echo
Run molecule test
[Pipeline] sh
+ molecule test
/home/jenkins/.local/lib/python3.6/site-packages/requests/__init__.py:104: RequestsDependencyWarning: urllib3 (1.26.20) or chardet (5.0.0)/charset_normalizer (2.0.12) doesn't match a supported version!
  RequestsDependencyWarning)
INFO     default scenario test matrix: create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
INFO     Guessed /opt/jenkins_agent/workspace/declarative pipeline as project root directory
INFO     Using /home/jenkins/.cache/ansible-lint/9a8524/roles/georgy.vector symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/home/jenkins/.cache/ansible-lint/9a8524/roles
INFO     Running default > create
INFO     Sanity checks: 'docker'
/home/jenkins/.local/lib/python3.6/site-packages/paramiko/transport.py:33: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.hazmat.backends import default_backend

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 2, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/quay.io/centos/centos:stream8)
skipping: [localhost] => (item=molecule_local/pycontribs/ubuntu:latest)
skipping: [localhost] => (item=molecule_local/pycontribs/centos:7)

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance)
changed: [localhost] => (item=ubuntu)
changed: [localhost] => (item=centos)

TASK [Wait for instance(s) creation to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '911518127998.19335', 'results_file': '/home/jenkins/.ansible_async/911518127998.19335', 'changed': True, 'failed': False, 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '954145950655.19361', 'results_file': '/home/jenkins/.ansible_async/954145950655.19361', 'changed': True, 'failed': False, 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '839986794662.19406', 'results_file': '/home/jenkins/.ansible_async/839986794662.19406', 'changed': True, 'failed': False, 'item': {'image': 'pycontribs/centos:7', 'name': 'centos', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
ok: [instance]
ok: [ubuntu]
ok: [centos]

TASK [Include vector] **********************************************************

TASK [georgy.vector : Create directory vector] *********************************
changed: [instance]
changed: [ubuntu]
changed: [centos]

TASK [georgy.vector : Get vector distrib] **************************************
changed: [centos]
changed: [ubuntu]
changed: [instance]

TASK [georgy.vector : Unarchive vector] ****************************************
changed: [instance]
changed: [ubuntu]
changed: [centos]

TASK [georgy.vector : Create a symbolic link] **********************************
changed: [centos]
changed: [instance]
changed: [ubuntu]

TASK [georgy.vector : Create vector unit flie] *********************************
changed: [instance]
changed: [ubuntu]
changed: [centos]

TASK [georgy.vector : Mkdir vector data] ***************************************
changed: [ubuntu]
changed: [centos]
changed: [instance]

TASK [georgy.vector : Vector config create] ************************************
changed: [centos]
changed: [ubuntu]
changed: [instance]

PLAY RECAP *********************************************************************
centos                     : ok=8    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
instance                   : ok=8    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=8    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
ok: [instance]
ok: [ubuntu]
ok: [centos]

TASK [Include vector] **********************************************************

TASK [georgy.vector : Create directory vector] *********************************
ok: [centos]
ok: [instance]
ok: [ubuntu]

TASK [georgy.vector : Get vector distrib] **************************************
ok: [centos]
ok: [ubuntu]
ok: [instance]

TASK [georgy.vector : Unarchive vector] ****************************************
skipping: [ubuntu]
skipping: [instance]
skipping: [centos]

TASK [georgy.vector : Create a symbolic link] **********************************
ok: [ubuntu]
ok: [instance]
ok: [centos]

TASK [georgy.vector : Create vector unit flie] *********************************
ok: [centos]
ok: [ubuntu]
ok: [instance]

TASK [georgy.vector : Mkdir vector data] ***************************************
ok: [centos]
ok: [instance]
ok: [ubuntu]

TASK [georgy.vector : Vector config create] ************************************
ok: [centos]
ok: [ubuntu]
ok: [instance]

PLAY RECAP *********************************************************************
centos                     : ok=7    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
instance                   : ok=7    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ubuntu                     : ok=7    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Gathering Facts] *********************************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
ok: [instance]
ok: [ubuntu]
ok: [centos]

TASK [Get Vector version] ******************************************************
ok: [instance]
ok: [ubuntu]
ok: [centos]

TASK [Assert Vector instalation] ***********************************************
ok: [centos] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [instance] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

TASK [Validation Vector configuration] *****************************************
ok: [ubuntu]
ok: [instance]
ok: [centos]

TASK [Assert Vector validate config] *******************************************
ok: [centos] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [instance] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos                     : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
instance                   : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
changed: [localhost] => (item=instance)
changed: [localhost] => (item=ubuntu)
changed: [localhost] => (item=centos)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=instance)
changed: [localhost] => (item=ubuntu)
changed: [localhost] => (item=centos)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS

```

4. Создать Multibranch Pipeline на запуск `Jenkinsfile` из репозитория.

```
Started by user admin
Running as SYSTEM
Building on the built-in node in workspace /var/lib/jenkins/workspace/multibranch pipeline
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential e04d7e0d-faee-40f4-9a9c-c6ea83fbb998
Cloning the remote Git repository
Cloning repository https://github.com/stepynin-georgy/vector-role.git
 > git init /var/lib/jenkins/workspace/multibranch pipeline # timeout=10
Fetching upstream changes from https://github.com/stepynin-georgy/vector-role.git
 > git --version # timeout=10
 > git --version # 'git version 1.8.3.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --progress https://github.com/stepynin-georgy/vector-role.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url https://github.com/stepynin-georgy/vector-role.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/tests^{commit} # timeout=10
Checking out Revision a9bf3313b725579e6860628844d5fead8698b1de (refs/remotes/origin/tests)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f a9bf3313b725579e6860628844d5fead8698b1de # timeout=10
Commit message: "Update Jenkinsfile"
First time build. Skipping changelog.
Triggering multibranch pipeline » default
multibranch pipeline » default completed with result SUCCESS
Finished: SUCCESS
```

5. Создать Scripted Pipeline, наполнить его скриптом из [pipeline](./pipeline).
6. Внести необходимые изменения, чтобы Pipeline запускал `ansible-playbook` без флагов `--check --diff`, если не установлен параметр при запуске джобы (prod_run = True). По умолчанию параметр имеет значение False и запускает прогон с флагами `--check --diff`.
7. Проверить работоспособность, исправить ошибки, исправленный Pipeline вложить в репозиторий в файл `ScriptedJenkinsfile`.

prod_run=False

```
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on agent in /opt/jenkins_agent/workspace/scripted pipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Git checkout)
[Pipeline] git
The recommended git tool is: NONE
No credentials specified
Fetching changes from the remote Git repository
Checking out Revision 20bd8d945340bb742acdd9e8c1a8fb5b73cc1700 (refs/remotes/origin/master)
Commit message: "Merge branch 'master' of https://github.com/aragastmatb/example-playbook"
 > git rev-parse --resolve-git-dir /opt/jenkins_agent/workspace/scripted pipeline/.git # timeout=10
 > git config remote.origin.url https://github.com/aragastmatb/example-playbook.git # timeout=10
Fetching upstream changes from https://github.com/aragastmatb/example-playbook.git
 > git --version # timeout=10
 > git --version # 'git version 1.8.3.1'
 > git fetch --tags --progress https://github.com/aragastmatb/example-playbook.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 20bd8d945340bb742acdd9e8c1a8fb5b73cc1700 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master 20bd8d945340bb742acdd9e8c1a8fb5b73cc1700 # timeout=10
 > git rev-list --no-walk 20bd8d945340bb742acdd9e8c1a8fb5b73cc1700 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Run playbook)
[Pipeline] sh
+ ansible-playbook site.yml -i inventory/prod.yml --ask-become-pass
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
/usr/lib64/python3.6/getpass.py:91: GetPassWarning: Can not control echo on the terminal.
  passwd = fallback_getpass(prompt, stream)
Warning: Password input may be echoed.
BECOME password: 
PLAY [Install Java] ************************************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [java : Upload .tar.gz file containing binaries from local storage] *******
skipping: [localhost]

TASK [java : Upload .tar.gz file conaining binaries from remote storage] *******
ok: [localhost]

TASK [java : Ensure installation dir exists] ***********************************
changed: [localhost]

TASK [java : Extract java in the installation directory] ***********************
changed: [localhost]

TASK [java : Export environment variables] *************************************
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   

[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

prod_run=True

```
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on agent in /opt/jenkins_agent/workspace/scripted pipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Git checkout)
[Pipeline] git
The recommended git tool is: NONE
No credentials specified
Fetching changes from the remote Git repository
Checking out Revision 20bd8d945340bb742acdd9e8c1a8fb5b73cc1700 (refs/remotes/origin/master)
Commit message: "Merge branch 'master' of https://github.com/aragastmatb/example-playbook"
 > git rev-parse --resolve-git-dir /opt/jenkins_agent/workspace/scripted pipeline/.git # timeout=10
 > git config remote.origin.url https://github.com/aragastmatb/example-playbook.git # timeout=10
Fetching upstream changes from https://github.com/aragastmatb/example-playbook.git
 > git --version # timeout=10
 > git --version # 'git version 1.8.3.1'
 > git fetch --tags --progress https://github.com/aragastmatb/example-playbook.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 20bd8d945340bb742acdd9e8c1a8fb5b73cc1700 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master 20bd8d945340bb742acdd9e8c1a8fb5b73cc1700 # timeout=10
 > git rev-list --no-walk 20bd8d945340bb742acdd9e8c1a8fb5b73cc1700 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Run playbook)
[Pipeline] sh
+ ansible-playbook site.yml -i inventory/prod.yml --ask-become-pass --check --diff
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
/usr/lib64/python3.6/getpass.py:91: GetPassWarning: Can not control echo on the terminal.
  passwd = fallback_getpass(prompt, stream)
Warning: Password input may be echoed.
BECOME password: 
PLAY [Install Java] ************************************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [java : Upload .tar.gz file containing binaries from local storage] *******
skipping: [localhost]

TASK [java : Upload .tar.gz file conaining binaries from remote storage] *******
ok: [localhost]

TASK [java : Ensure installation dir exists] ***********************************
ok: [localhost]

TASK [java : Extract java in the installation directory] ***********************
skipping: [localhost]

TASK [java : Export environment variables] *************************************
ok: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   

[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

8. Отправить ссылку на репозиторий с ролью и Declarative Pipeline и Scripted Pipeline.
9. Сопроводите процесс настройки скриншотами для каждого пункта задания!!

## Необязательная часть

1. Создать скрипт на groovy, который будет собирать все Job, завершившиеся хотя бы раз неуспешно. Добавить скрипт в репозиторий с решением и названием `AllJobFailure.groovy`.
2. Создать Scripted Pipeline так, чтобы он мог сначала запустить через Yandex Cloud CLI необходимое количество инстансов, прописать их в инвентори плейбука и после этого запускать плейбук. Мы должны при нажатии кнопки получить готовую к использованию систему.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
