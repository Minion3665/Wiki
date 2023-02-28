At some point I would like to write some nix code in order to deploy clicks server reproducibly, here are the options:
| Option          | Advantages                                                  | Disadvantages                                                                    |
| --------------- | ----------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Nixops          | Seems to be somewhat standard                               | Broken in nixpkgs, unstable version is lacking features/docs                     |
| Bento           | Looks nice and simple                                       | Requires an always-on server, no spinning up machines, poor flake update support |
| colmena         | Works with flakes, supports filesystems normally            | No spinning up new machines on the fly (e.g. w/ aws)                             |
| deploy-rs       | ditto as to colmena, if we choose it more comparison needed | ditto as to colmena, if we choose it more comparison needed                      |
| krops           | secret storage built in (we would use sops)                 | maybe not feature rich enough, definitely no spinning up new machines on the fly |
| kubenix         |                                                             | Kubernetes                                                                       |
| kubernix        |                                                             | Kubernetes                                                                       |
| morph           |                                                             |                                                                                  |
| nixery          | Docker containers are OCI standard, maintained by TVL       | very cool, absolutely not what we're looking for but this seems overall awesome  |
| nixinate        |                                                             | "Nixinate is a proof of concept"                                                 |
| pushnix         |                                                             | better options out there imo                                                     |
| terraform-nixos |                                                             | opposite of what we need? maybe useful alongside something else                  |
| terranix        | Terraform is a standard, building with nix is an advantage  | Terraform                                                                        |

Realistically, let's just use nixops *unless* you cannot manage filesystems in it properly. It is expected to continue development and has some stability even on unstable so long as we are on a nixos stable release. Nixops must also be entirely stateless which some reports say it is not.