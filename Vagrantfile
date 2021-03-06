Vagrant.configure('2') do |config|
  config.vm.box = 'precise64'
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

  config.ssh.forward_agent = true

  config.vm.provision 'shell', inline: <<-'SH'
    set -e -x
    update-locale LC_ALL=en_US.UTF-8
    aptitude -q -y update
    aptitude -y install make
    if ! ruby -v | grep -F -q 2.1.0p0; then
      test -f ruby-2.1.0.tar.bz2 ||
        wget -q cache.ruby-lang.org/pub/ruby/2.1/ruby-2.1.0.tar.bz2
      test -f ruby-2.1.0.tar ||
        bunzip2 -k -q ruby-2.1.0.tar.bz2
      test -d ruby-2.1.0 ||
        tar -x -f ruby-2.1.0.tar
      cd ruby-2.1.0
      ./configure --disable-install-doc
      make
      make install
      cd ..
    fi
    gem update --no-document --system 2.1.11
  SH

  config.vm.provision 'shell', inline: <<-'SH', privileged: false
    set -e -x
    echo '{ gem: --no-document, install: --user-install }' > .gemrc
    echo $'PATH="$(ruby -e \'puts Gem.user_dir\')/bin:$PATH"' > .bash_profile
    . .bash_profile
    gem install bundler
    cd /vagrant
    bundle install
  SH
end
