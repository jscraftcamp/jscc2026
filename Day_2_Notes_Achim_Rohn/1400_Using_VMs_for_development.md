# Using VMs for development

The goal is to have an isolated development environment for in the case of your environment becoming compromised, you at least contain inside the VM and it doesn't affect your host machine as well. By doing that, you protect your most sensitive data.

The focus of the session was on how to quickly set up these VMs.

On Linux you can create VM definitions for example for QEMU that allow you to quickly spin up new VMs.

You can then use tools like Nix/NixOS, Ansible, Terraform, Pulumi or even just shell scripts to quickly set up the contents inside the VM.