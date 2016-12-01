# Packer templates for Ubuntu (revisited by Quarkslab)

See original repo here: https://github.com/boxcutter/ubuntu.


1. [Boxes informations](#boxes-informations)
    1. [Ubuntu Server 16.10 AMD64](#ubuntu-server-16.10-amd64)
    1. [Ubuntu Server 16.04 AMD64](#ubuntu-server-16.04-amd64)
1. [Generate Boxes](#generate-boxes)
1. [Security](#security)


## Boxes informations

See https://atlas.hashicorp.com/quarkslab/boxes.


### Ubuntu Server 16.10 AMD64

See https://atlas.hashicorp.com/quarkslab/boxes/ubuntu-16.10-server-amd64.

Test it:
```bash
    $ vagrant init quarkslab/ubuntu-16.10-amd64
    $ vagrant up --provider vmware_workstation
```

| Provider       | Version  | Atlas link                       | SHA256                                                           |
| :------:       | :-----:  | :--------:                       | :----:                                                           |
| VMware_desktop | 20161130 | [link][16.10-amd64-server-amd64] | f60835c1e7b5ffa89050421da439f896e9ff26f034c8e65a487881c788f661eb |

[16.10-amd64-server-amd64]: https://atlas.hashicorp.com/quarkslab/boxes/ubuntu-16.10-server-amd64/versions/20161130/providers/vmware_desktop.box


### Ubuntu Server 16.04 AMD64

See https://atlas.hashicorp.com/quarkslab/boxes/ubuntu-16.04-amd64.

Test it:
```bash
    $ vagrant init quarkslab/ubuntu-16.04-amd64
    $ vagrant up --provider vmware_workstation
```

| Provider       | Version  | Atlas box link               | Signature                        | SHA256                                                           |
| :------:       | :-----:  | :------------:               | :-------:                        | :----:                                                           |
| VMware_desktop | 20160916 | [link][16.04-amd64-20160916] | [link][16.04-amd64-20160916.sig] | f40ac77251c62509c68265e9457a1ab8244d6df6fcc57361a829355bd5a2afe1 |

[16.04-amd64-20160916]: https://atlas.hashicorp.com/quarkslab/boxes/ubuntu-16.04-amd64/versions/20160916/providers/vmware_desktop.box
[16.04-amd64-20160916.sig]: box/vmware/ubuntu-16.04-amd64-20160916.box.sig


## Generate Boxes

Don't forget to download in the directory the ISO file listed in the ubuntu-XX.XX-amd64.json file.

If you want to automatically upload the Vagrant box you’ve created to the [Atlas
platform](https://atlas.hashicorp.com), check the `ubuntu.atlas.json`.

Example:
```bash
    $ export ATLAS_TOKEN=<your-atlas-token> # See https://atlas.hashicorp.com/help/user-accounts/authentication
    $ packer-io build -only=vmware-iso -var-file=ubuntu1610.json -var 'atlas_username=quarkslab' -var 'atlas_box_name=ubuntu-16.10-server-amd64' ubuntu.atlas.json
```

Modify the README and sign it:
```bash
    $ gpg --output README.md.sig --detach-sig README.md
```


## Security

First, verify the authenticity of the README.md file using PGP.

```bash
    $ gpg --recv-keys 24CF4A6F
    $ gpg --verify README.md.sig README.md
```

Now, you can take advantage of the `config.vm.box_download_checksum`
[Vagrantfile
option](https://docs.vagrantup.com/v2/vagrantfile/machine_settings.html).

An example of Vagrantfile:
```
    […]
    config.vm.box_url = "https://atlas.hashicorp.com/quarkslab/boxes/ubuntu-16.04-amd64/versions/20160916/providers/vmware_desktop.box"
    config.vm.box_download_checksum = "f40ac77251c62509c68265e9457a1ab8244d6df6fcc57361a829355bd5a2afe1"
    config.vm.box_download_checksum_type = "sha256"
    […]
```
