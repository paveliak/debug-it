# debug-it

This action allows connecting via SSH to the Actions runner using DevTunnels. For MacOS and Windows runners you can also connect via VNC and RDP respectively (in addition to SSH).
GitHub credentials are used to authenticate to a devtunnel scoping access to only the user that does the debugging.

**NOTE**: Be cautious when using this action with self-hosted runners as the GitHub creadentials you use wtihin the runner to create a tunnel could get exposed to other workloads on the machine. 

Example usage:

1. Add this yaml into your workflow. When job fails you can can [rerun it with debugging enabled](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/re-run-workflows-and-jobs#re-running-failed-jobs-in-a-workflow) and that would execute the debugging step
```yaml
permissions:
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: |
          ...

      - name: Debug failed job
        uses: paveliak/debug-it@main
        if: ${{ failure() && runner.debug == 1 }}
```

2. Copy/paste DevTunnel id from the Actions job log and connect to it with `devtunnel connect <devtunnel-id>`
3. Observe which local port was forwarded (suppose 55555), open another terminal window and connect with `ssh -p 55555 runner@127.0.0.1` (on Windows the user is `runneradmin`)
