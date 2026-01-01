# debug-it

This action allows connecting via SSH to the Actions runner using DevTunnels. For MacOS and Windows runners you can also connect via VNC and RDP respectively (in addition to SSH).
GitHub credentials are used to authenticate to a devtunnel scoping access to only the user that does the job debugging. Use `devtunnel login --github` to authenticate locally.

**NOTE**: Be cautious when using this action with self-hosted runners as the GitHub credentials you use wtihin the runner to create a tunnel could get exposed to other workloads on the machine. 

When accessing a runner via RDP/VNC you would need to provide a password to a remote machine. This action resets VM password to the one you specify in the (optional) `new-password` parameter. Password needs to be strong enough or attempt to reset it might throw an exception. You do not need providing this parameter for the SSH access as it relies on the public key authentication. Public key is retrieved from the `https://github.com/<user>.keys` endpoint so make sure you [add SSH public key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

## Example usage:

1. Add this yaml into your workflow. When job fails you can can [rerun it with debugging enabled](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/re-run-workflows-and-jobs#re-running-failed-jobs-in-a-workflow) and that would execute the debugging step. Authenticate to DevTunnel using the device code provided in the job log.
```yaml
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

2. Copy/paste DevTunnel id from the Actions job log and connect to it with `devtunnel connect <devtunnel-id>` from your local machine
3. Observe which local port was forwarded (suppose 55555), open another terminal window and connect with `ssh -p 55555 runner@127.0.0.1` (on Windows the user is `runneradmin`)
4. For the RDP and VNC connect to 127.0.0.1:3389 and 127.0.0.1:5900 respectively
