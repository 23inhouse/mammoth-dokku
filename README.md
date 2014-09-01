# Dokku install
## For a Rails app running on Mammoth VPS

This is a scripted version of the pain I went through to get it working.

AKA - It works on my machine TM*

## Installation

Add this line to your application's Gemfile:

    git clone git://github.com/23inhouse/mammoth-dokku

Then:

    # copies the files over and secures your ssh
    $ ./run_me vps <your-host>
    $ ssh -p22 <your-name>@<your-host>
    $ sudo ~/setup/remote_1 <your-name>
    $ exit


And:

    # locally add the new host
    $ vim ~/.ssh/config

    # add your host

    Host <your-host>
        HostName <your-host>
        Port 30000
        User <your-name>

And:

    # adds a swap file and installs docker
    $ ssh <your-host>
    $ sudo ~/setup/remote_2

    # installs dokku
    $ ssh <your-host>
    $ sudo ~/setup/remote_3 <your-name>

And:

    # now for the pay off
    # run these locally
    $ cd ~/git/<your-app>
    # add the 'dokkufy' gem to your project
    # add "gem 'dokkufy'""
    $ vim Gemfile && bundle
    $ bundle exec dokkufy app
    $ git push dokku master
    $ git push dokku master # Dokku = Works on my machine TM

Extra:

    # PostgreSQL
    # run these locally
    $ cd ~/git/<your-app>
    $ bundle exec dokkufy plugin:list
    $ bundle exec dokkufy plugin:install 1 <your-host> <your-name>
    $ bundle exec dokkufy plugin:install 4 <your-host> <your-name>
    $ ssh <your-host>
    $ sudo dokku postgresql:create <your-app>
    # copy the info into your rails app
    $ sudo docker ps -a
    $ sudo docker run -i -t dokku/<your-app>:latest /bin/bash
    $ cd app/
    $ export HOME=/app
    $ for file in /app/.profile.d/*; do source $file; done
    $ hash -r
    $ cd /app
    $ RAILS_ENV=production rake db:migrate
    # run locally
    $ git push dokku master

### Farq! It didn't work as advertised:

    # ssh <your-host>
    $ dokku logs <your-app>
    $ dokku run <your-app> bundle exec rails c production
    $ docker ps -a

