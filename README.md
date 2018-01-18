# Puppet Code Manager CLI

This project provides a utility script to interact with Puppet Enterprise's
Code Manager in a variety of ways which may be useful either during debugging
or automated bootstrapping of Puppet Enterprise.

This script MUST be run as root, from the machine running code manager.

Because the code manager service has a dependency on the RBAC service and an
authentication token, this script does not actually invoke code manager
directly. It instead calls independent subsystems to orchestrate equivalent
work.

The script is intended to be used only in situations where for whatever reason
code manager's all-or-nothing invocations are insufficient to solve a problem,
or in an automation situation where it is not realistic to involve the RBAC
service or a token.

## Usage

```
Usage: puppet-code-manager [-v] <subcommand>

Available subcommands:

  deploy <environment>
    Roughly equivalent to "puppet code deploy <env>". Invokes r10k to
    deploy the environment to the staging-dir, invokes file-sync to
    commit and to sync the code-dir.

  commit
    Commits all files in code-staging and deploys them.

  r10k <arguments>
    Invokes r10k in the context of code manager. Normal r10k arguments
    should be provided.

  file-sync <subcommand>
    Run the file-sync subcommand with no other options for further
    usage instructions.

  flush-environment-cache
    Flush the puppetserver environment cache, causing it to re-read code
    from the code-dir.
```

### `file-sync` subcommand

```
Usage: puppet-code-manager [-v] file-sync <subcommand>

Available subcommands:

  commit
    Calls the file-sync storage API to read and commit all code in
    the staging-dir. Does not force the client to sync or flush
    the puppetserver environment cache.

  force-sync
    Calls the file-sync client API to force the code-dir to be updated
    with the latest available code from the storage service.
```
