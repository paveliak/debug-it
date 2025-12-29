# debug-it

This action allows connecting via SSH (VNC/RDP) to the Actions runner using DevTunnels.
Entra Auth is used to create tunnel (You can create free Azure account and Entra is free).
GitHub credentials are accepted by DevTunnels too and Entra was picked as it allows non-interactive auth.

Authentication uses OIDC so your Entra app needs to have federated credentials configured.

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
        with:
          tenant-id: ${{ secrets.TENANT_ID }}
          client-id: ${{ secrets.CLIENT_ID }}
```

2. Copy/paste DevTunnel id from the Actions job log and connect to it with `devtunnel connect <devtunnel-id>`
3. Observe which local port was forwarded (suppose 55555), open another terminal window and connect with `ssh -p 55555 runner@127.0.0.1` (on Windows the user is `runneradmin`)
