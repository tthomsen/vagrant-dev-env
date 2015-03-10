# vagrant-dev-env
# puppet-master-development
Creates a simple Ubuntu dev environment that is configured using Puppet.  If you want to use a different flavor goto https://vagrantcloud.com/puppetlabs/boxes/ for additional box files.
Edit vagrant.yml to change resources and add additional nodes.  Make sure to copy the provisioners section for any additional nodes.  This is what tells vagrant to use Puppet to configure your node.

```
boxes:
  puppetlabs/ubuntu-14.04-64-puppet: "puppetlabs/ubuntu-14.04-64-puppet"
nodes:
  web:
    hostname: web.vagrant.vm
    box: puppetlabs/ubuntu-14.04-64-puppet
    memory: 1024
    cpus: 1
    networks:
      - private_network:
          ip: 192.168.137.16
    provisioners:
      - puppet:
          manifests_path: "puppet/manifests"
          manifest_file: "site.pp"
          module_path: "puppet/modules"
          working_directory: "/vagrant/puppet/manifests"
          options: "--verbose --debug"
    synced_folders:
      - host: .
        guest: /vagrant
```

Before preforming your vagrantup you will need to download any Puppet modules(https://forge.puppetlabs.com/)  into puppet/modules and configure your site.pp.

sample site.pp
```
node 'web', 'web.vagarnt.vm' {
  class { 'apache': }
}
```
Bring up the environments.
```
vagrant up
```
