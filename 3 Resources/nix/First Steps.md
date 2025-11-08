https://nix.dev/tutorials/first-steps/ad-hoc-shell-environments

## Create a shell env

Use `nix-shell` with the `-p` (`--packages`) option to specify that we need the `cowsay` and `lolcat` packages:

```
nix-shell -p cowsay lolcat
```

```
[nix-shell:~]$ cowsay Hello, Nix! | lolcat
```

## Running programs once

```
nix-shell -p cowsay --run "cowsay Nix"
```

## Packages
```
https://search.nixos.org/packages
```


## Clean up
```
nix-collect-garbage
```

## Reproducible interpreted scripts

Create a file named `nixpkgs-releases.sh` with the following content:

```
#!/usr/bin/env nix-shell
#! nix-shell -i bash --pure
#! nix-shell -p bash cacert curl jq python3Packages.xmljson
#! nix-shell -I nixpkgs=https://github.com/NixOS/nixpkgs/archive/2a601aafdc5605a5133a2ca506a34a3a73377247.tar.gz

curl https://github.com/NixOS/nixpkgs/releases.atom | xml2json | jq .
```

The first line is a standard shebang. The additional shebang lines are a Nix-specific construct:

- With the `-i` option, `bash` is specified as the interpreter for the rest of the file.
    
- In this case, the `--pure` option is enabled to prevent the script from implicitly using programs that may already exist on the system on which the script is run.
    
- The `-p` option lists the packages required for the script to run.
    
    The command `xml2json` is provided by the package `python3Packages.xmljson`, while `bash`, `jq`, and `curl` are provided by packages of the same name. `cacert` must be present for SSL authentication to work.
    

```
chmod +x nixpkgs-releases.sh
```

```
./nixpkgs-releases.sh
```

nix-shell reference - https://nix.dev/manual/nix/stable/command-ref/nix-shell.html#options
