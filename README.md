# debug-it

This action allows connecting via SSH (VNC/RDP) to the Actions runner using DevTunnels.
Entra Auth is used to create tunnel (You can create free Azure account and Entra is free).
GitHub credentials are accepted by DevTunnels too and Entra was picked as it allows non-interactive auth.

Authentication uses OIDC so your Entra app needs to have federated credentials configured.

Example usage:

```yaml
permissions:
  id-token: write

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: paveliak/debug-it@main
        with:
          tenant-id: ${{ secrets.TENANT_ID }}
          client-id: ${{ secrets.CLIENT_ID }}
```