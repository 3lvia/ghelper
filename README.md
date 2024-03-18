# ghelper

A simple script to help with the management of multiple GitHub repositories.

## Pre-requisites

`git` and `gh` are required to be installed and available in the PATH.

You must also have authorized your GitHub account with `gh` by running `gh auth login`.

## Installation

```bash
git clone git@github.com:3lvia/ghelper.git
```

```bash
cd ghelper
```

```bash
./ghelper
```

## Usage

```bash
$ ./ghelper --help
```

```
Usage: ghelper [subcommand] [options]

Subcommand:

commit: Create a new branch, add/delete files, and open/merge a pull request.
exec:   Execute a shell command in multiple git repositories.

-h, --help: Show this help message.
```

### Commit

```bash
$ ./ghelper commit --help
```

```
Usage: ghelper commit <list of git repositories>

Options:

  -a, --add <src1=dest1,src2=dest2,src3=dest3,...>   Files to add. Source files are assumed to be in the same directory as the script is run from.
  -d, --delete <file1,file2,...>                     Files to delete.
  -b, --new-branch-name <name>                       Name of the branch to create.
  -m, --commit-message <message>                     Commit message.
  -c, --continue-on-missing-file                     Continue even if a file to delete is missing.
  -B, --base-branch-name <name>                      Name of the base branch to create the new branch from. If not provided, the script will try to find a main branch (main, master, trunk, develop) in the repository.
  -P, --no-push                                      Do not push to origin.
  -O, --no-pull-request                              Do not open a pull request.
  -M, --merge                                        Automatically merge pull requests. Uses merge commit by default, can be combined with --squash or --rebase.
  -S, --squash                                       Squash commits when merging pull requests.
  -R, --rebase                                       Rebase commits when merging pull requests.
  -C, --no-clean                                     Do not clean up working directory.
  -h, --help                                         Show this help message.
  --debug                                            Enable debug mode.

Examples:

1. Create a new branch and add a file to it:
   ghelper commit --add renovate.json=.github/renovate.json --new-branch-name ci/add-renovate 3lvia/cool-system

2. Create a new branch, add multiple files to it, and delete another file:
   ghelper commit --add yarn.lock=frontend/yarn.lock,package.json=frontend/package.json --delete frontend/package-lock.json --new-branch-name build/use-yarn 3lvia/cool-system

3. Create a new branch, add a file to it, use a custom commit message and automatically merge using squash:
   ghelper commit --add .github/workflows/ci.yml --new-branch-name ci/add-ci --commit-message 'Add CI workflow' --merge --squash 3lvia/cool-system

4. Create a new branch, add a file to it, and do not push to origin using repository list file:
   ghelper commit --add .github/workflows/ci.yml --new-branch-name ci/add-ci --no-push repositories.txt
```

### Exec

```bash
$ ./ghelper exec --help
```

```
Usage: ghelper exec [options] <list of git repositories>

Options:

  -x, --exec <command>        Command to execute.
  -l, --log-file <file>       Log file to write output to.
  -b, --branch <name>         Name of the branch to checkout.
  -C, --no-clean              Do not clean up working directory.
  -h, --help                  Show this help message.
  --debug                     Enable debug mode.

Examples:

1. Check if the file 'README.md' exists in the main branch of the repository '3lvia/ghelper':
   ghelper exec -x 'ls README.md' 3lvia/ghelper

2. Run a Trivy scan on the repositories '3lvia/core-terraform' and '3lvia/runtimeservice-terraform':
   ghelper exec -x 'trivy config --severity HIGH .' 3lvia/core-terraform,3lvia/runtimeservice-terraform
```
