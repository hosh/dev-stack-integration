Vagrant::Config.run do |config|
  # The path to the Berksfile to use with Vagrant Berkshelf
  # config.berkshelf.berksfile_path = "./Berksfile"

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to skip installing and copying to Vagrant's shelf.
  # config.berkshelf.only = []

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to skip installing and copying to Vagrant's shelf.
  # config.berkshelf.except = []

  config.vm.host_name = "dev-stack-integration"

  # Build this from https://github.com/opscode/bento
  config.vm.box = "ubuntu-12.04"
  config.vm.box_url = "https://opscode-vm.s3.amazonaws.com/vagrant/opscode_ubuntu-12.04_chef-11.2.0.box"

  config.vm.customize do |vm|
    vm.memory_size = 512
  end

  config.vm.network :hostonly, "33.33.33.120"
  # config.vm.network :bridged

  config.ssh.max_tries = 40
  config.ssh.timeout   = 120

  config.vm.provision :chef_solo do |chef|
    chef.data_bags_path = "data_bags"
    chef.json = {
      'dev-stack' => {
        app: { root_dir: '/vagrant/apps/dev-stack-rails-demo' },
        rails: {
          postgresql: {
            development_name: 'dev-stack-rails-demo_development',
            test_name:        'dev-stack-rails-demo_test',
            username:         'dev-stack-rails-demo',
            password:         ''
          } # postgresql
        } # rails
      }, # dev-stack

      build_essential: { compiletime: true },
      nginx: { default_site_enabled: false },
      postgresql: {
        password: { postgres: 'rails' },

        # This is a dev box. Trust everything
        pg_hba: [
          { type: 'local', db: 'all', user: 'postgres', addr: nil, method: 'trust' },
          { type: 'local', db: 'all', user: 'all',      addr: nil, method: 'trust' }
        ]
      }
    }

    chef.run_list = [ "dev-stack-rails" ]
  end
end
