# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.

# Bandit is a security linter designed to find common security issues in Python code.
# This action will run Bandit on your codebase.
# The results of the scan will be found under the Security tab of your repository.

# https://github.com/marketplace/actions/bandit-scan is ISC licensed, by abirismyname
# https://pypi.org/project/bandit/ is Apache v2.0 licensed, by PyCQA

name: Bandit
on:
  push:
    branches: [ "main" ]
  pull_request:
    # The branches below must be a subset of the branches above
    branches: [ "main" ]
  schedule:
    - cron: '33 21 * * 3'

jobs:
  bandit:
    permissions:
      contents: read # for actions/checkout to fetch code
      security-events: write # for github/codeql-action/upload-sarif to upload SARIF results
      actions: read # only required for a private repository by github/codeql-action/upload-sarif to get the Action run status

    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Bandit Scan
        uses: shundor/python-bandit-scan@9cc5aa4a006482b8a7f91134412df6772dbda22c
        
        with: # optional arguments
          # exit with 0, even with results found
          exit_zero: true # optional, default is DEFAULT
          # Github token of the repository (automatically created by Github)
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # Needed to get PR information.
          # File or directory to run bandit on
          # path: # optional, default is .
          # Report only issues of a given severity level or higher. Can be LOW, MEDIUM or HIGH. Default is UNDEFINED (everything)
          # level: # optional, default is UNDEFINED
          # Report only issues of a given confidence level or higher. Can be LOW, MEDIUM or HIGH. Default is UNDEFINED (everything)
          # confidence: # optional, default is UNDEFINED
          # comma-separated list of paths (glob patterns supported) to exclude from scan (note that these are in addition to the excluded paths provided in the config file) (default: .svn,CVS,.bzr,.hg,.git,__pycache__,.tox,.eggs,*.egg)
          # excluded_paths: # optional, default is DEFAULT
          # comma-separated list of test IDs to skip
          # skips: # optional, default is DEFAULT
          # path to a .bandit file that supplies command line arguments
          # ini_path: # optional, default is DEFAULT
