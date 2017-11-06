## Example project: Nix for PureScript development

### Building: one-liner

With Nix installed, the following command will install system dependencies (NodeJS), install project dependencies (NPM and PureScript packages), and build the app:

```
nix-build
```

To run it:

```
./result/bin/purescript-nix-bootstrap
```

### Building: development

1) Install Nix from [here](https://nixos.org/nix/).

The only dependency you install globally on your system is the Nix package manager. Everything else is installed in a temporary sandbox by Nix as needed. Make sure the Nix binaries are on your path, e.g. by adding `~/.nix-profile/bin` to your path.

2) Install PureScript + NodeJS + any other system dependencies, then drop into a shell with those installed:

```
nix-shell
```

3) Install project dependencies (PureScript and NPM packages):

```
npm install
./bower update --force-latest
```

4) Build project, rebuilding on file changes:

```
npm run build:watch
```

### Modifying system dependencies

Add any system dependencies you need to `buildInputs` of the file `default.nix`, then open a new Nix shell with `nix-shell` as above.

### Updating all system dependencies

Find the hash of the commit you want from the nixpkgs-channels repository (https://github.com/nixos/nixpkgs-channels), then dump it to `nixpkgs.json` with the following command (substituting the hash for the one you want):

```
nix-prefetch-git https://github.com/nixos/nixpkgs-channels --rev cfafd6f5a819472911eaf2650b50a62f0c143e3e > nixpkgs.json
```

### Running in Docker

Build a Docker image:

```
nix-build docker.nix
```

Load image into Docker locally:

```
docker load -i result
```

Run image:
```
docker run -it --rm --name purescript-nix-bootstrap -p 3000:3000 purescript-nix-bootstrap
```