# Packer templates for Ubuntu (revisited by Quarkslab)

See original repo here: https://github.com/boxcutter/ubuntu.


1. [Boxes informations](#boxes-informations)
    1. [Debian 8.2.0 AMD64](debian-8.2.0-amd64)
    2. [Debian 7.9.0 AMD64](debian-7.9.0-amd64)
2. [Generate Boxes](#generate-boxes)
3. [Security](#security)
    1. [SHA256](#sha256)
    2. [PGP](#pgp)


## Boxes informations

See https://atlas.hashicorp.com/quarkslab/boxes.


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
    $ packer-io build -only=virtualbox-iso -var-file=ubuntu1604.json ubuntu.atlas.json
```

### Troubleshooting

Be careful with VMWare provider, they may need a manual task to do:
- When the mirror selection fail, click on "Go back"
- Go back to the "selection step menu"
- Select the "Configure Network" and then it should not fail again


## Security

### SHA256

To improve security, you can take advantage of the
``config.vm.box_download_checksum`` [Vagrantfile
option](https://docs.vagrantup.com/v2/vagrantfile/machine_settings.html).

First, you need to verify the integrity of the checksums. As they are part of
the `README.md` file, you need to verify the integrity of this file (See [Security/PGP](#pgp)):

An example of Vagrantfile:
```
[…]
    config.vm.box_url = "https://atlas.hashicorp.com/quarkslab/boxes/ubuntu-16.04-amd64/versions/20160916/providers/vmware_desktop.box"
    config.vm.box_download_checksum = "f40ac77251c62509c68265e9457a1ab8244d6df6fcc57361a829355bd5a2afe1"
    config.vm.box_download_checksum_type = "sha256"
[…]
```


### PGP

To verify box integrity of downloaded boxes (located in `~/.vagrant.d/boxes`)
or integrity of the README.md (for SHA256 informations):
```bash
    $ gpg --recv-keys 24CF4A6F
    $ gpg --verify <signature_file> <file_to_verify>

    $ gpg --verify README.md.sig
```
