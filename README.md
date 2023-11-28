<h1 align="center">GitHub Actions self-hosted Runner Ansible role</h1>

An Ansible role that installs and configures GitHub Actions self-hosted Runners
inside one or multiple hosts, you can re-use it for many different URLs
(repositories or organizations) inside the same host in order to re-use it as
much as possible.

Main goals of this role:

- _avoid waste_: re-use the same host to provide a build environment for
  multiple repositories or organizations;
- _idempotence_: executing the role many times won't make anything break, steps
  have checks that validate whether or not they should be executed;

## Variables

For an exhaustive list of variables check the [defaults](defaults/main.yaml)
file. Ideally, all values will have commentaries describing what are their
purposes and by the default value you can tell the type.

### Required variables

Following values are required since there is no way to register the self-hosted
Runner without them

| Name                   | Description                                        |
| ---------------------- | -------------------------------------------------- |
| gh_runner_config_url   | GitHub Repository or Organization URL              |
| gh_runner_config_token | GitHub Registration token to authenticate the host |

## Example Playbook

Simplest use case: Single repository configuration on one host.

```yaml
- hosts: all
  vars:
    gh_runner_config_labels:
      - linux
      - self-hosted

    gh_runner_config_url: https://github.com/macunha1/ansible-github-actions-runner
    gh_runner_config_token: AC5TNLJP9SBAFNEKKLLBLF264J8XO

  roles:
    - role: gh_runner
      # become: yes
      tags: [gh_runner]
```

Complex use case to which this role was created for

```yaml
- hosts: foo
  roles:
    - role: macunha1.github_actions_runner
      vars:
        gh_runner_config_labels:
          - linux
          - self-hosted

        gh_runner_config_url: https://github.com/macunha1/ansible-github-actions-runner
        gh_runner_config_token: AC5TNLJP9SBAFNEKKLLBLF264J8XO

    - role: macunha1.github_actions_runner
      vars:
        gh_runner_config_url: https://github.com/macunha1/another-repository
        gh_runner_config_token: AC5CQV3IJRR2OAFGEFCPJ0WJPJQXO

    - role: macunha1.github_actions_runner
      vars:
        gh_runner_config_url: https://github.com/macunha-acme-corp
        gh_runner_config_token: ACYWUR9MHGR9U58C34W9ZK00UNBF
```

Note that despite using the same host, each one of these GitHub Actions Runner
configuration will have its own path and credentials. Therefore, they can live
well in harmony without killing each other.

## Sources : 

- [macunha1 original repository](https://github.com/macunha1/ansible-github-actions-runner)