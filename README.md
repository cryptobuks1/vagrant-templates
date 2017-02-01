# Vagrant Templates

The point of this repository is to hold `Vagrantfile` templates that I personally use as starting points for self-contained development/testing environments.

These are not minimal templates. They include configuration tweaks, **workarounds** for common issues that I bumped into, and provisioning scripts that install a few extra packages and customize the shell environment a bit. Check the appropriate `Vagrantfile` and the `vagrant/provision.sh` script, they should be fairly easy to modify. Some usage examples:

  * Use them _as is_ to spin up readily usable VMs where you can log into and test random stuff.
  * Add the necessary steps to provision your application inside the VM, maybe removing some redundant things.
  * Just use them as a reference to write your own minimal environments with tweaked settings.

## Dependencies

You'll need [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/) (obviously) with the `vagrant-vbguest` plugin installed. You can also install the `vagrant-reload` plugin, but it is **optional** (if you enable automatic installation of system updates, it allows the VM to be immediately rebooted after provisioning).

## Notes

By default, the `vagrant-vbguest` plugin tries to install/update the VirtualBox Guest Additions on every `vagrant up`. I find this annoying and recommend you to disable this behavior by adding something like the following to your `~/.vagrant.d/Vagrantfile`:
```ruby
Vagrant.configure(2) do |config|
    ...

    if Vagrant.has_plugin?("vagrant-vbguest")
        config.vbguest.auto_update = false
    end

    ...
end
```
The templates that need to install/update the VirtualBox Guest Additions already (re)enable `auto_update` explicitly.

On my older Macbook Pro the (VM) clocks drift quite significantly with paravirtualization enabled, and I never quite figured out how to fix it. On newer machines this is no longer the case so, if you're bothered by the fact that all of these templates disable paravirtualization, just remove the following line:
```ruby
vm.customize ["modifyvm", :id, "--paravirtprovider", "legacy"]
```
For these use-cases, I never noticed any performance degradation, but still...
