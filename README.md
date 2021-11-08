# update-flake-lock-test

This repository is a template for testing changes to the
https://github.com/DeterminateSystems/update-flake-lock/ action.

## Usage

1. Click "Use this template"
1. Give it a name, and click "Create repository from template"
1. Modify the `.github/workflows/update.yml` file to point to your fork's `update-flake-lock` repository and branch
1. Go to the "Actions" tab and click on "update-flake-lock"
1. Then click the "Run workflow" dropdown
1. Finally, click the "Run workflow" button

At this point, your action will be running against a flake.nix with an outdated
flake.lock in a simulation of a real-word use case. If the action succeeds, you
should see a PR appear against that repository.

### Example

If your username / organization is `some-name` and your branch is `some-branch`,
your `update.yml` would look like the following:

```yaml
name: update-flake-lock
on:
  workflow_dispatch:

jobs:
  lockfile:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Install Nix
        uses: cachix/install-nix-action@v14
        with:
          install_url: https://nixos-nix-install-tests.cachix.org/serve/vij683ly7sl95nnhb67bdjjfabclr85m/install
          install_options: '--tarball-url-prefix https://nixos-nix-install-tests.cachix.org/serve'
          extra_nix_config: |
            experimental-features = nix-command flakes
            access-tokens = github.com=${{ secrets.GITHUB_TOKEN }}
      - name: Update flake.lock
        uses: some-name/update-flake-lock@some-branch
```
