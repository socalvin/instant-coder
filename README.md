# What is instant coder?
Instant coder a very simple wrapper of Vagrant and Chef Solo. People do a lot of RoR development on MacOS (or windows?) but when it comes to a more serious applicaiton to be deployed on AWS or other cloud provides, it is better to have an identical environment all the way from development to production. This project is to let coder to start their first and typical RoR development enviornment on Ubuntu (xubuntu in this project as it is more lightweighted) using VirtualBox, Vagrant and Chef Solo technologies.

## IMPORTANT: Before you proceed, please make sure...
* You have stable and fast enough internet connection.
* You have time for Chef to finish the entire process, which can take 20 to 30 minutes to finish as it requires to download and compile various components in the software stack.
* You don't set a passphrase for your SSH key in your MacOS because the entire process is CLI-based and it does not bring up a GUI and asks you for the password. You can check it by following this [instruction](http://unix.stackexchange.com/questions/120424/is-there-a-way-to-check-a-users-ssh-key-to-see-if-the-passphrase-is-blank). You can remove the passphrase by "ssh-keygen -p" or read this [instruction](http://stackoverflow.com/questions/112396/how-do-i-remove-the-passphrase-for-the-ssh-key-without-having-to-create-a-new-ke) for details.

## First, let's set up the essential software in your MacOS
* Install Homebrew. Check it by "brew -v".
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

* Install RVM (or rbenv, whichever you like). Check it by "rvm -v".
```
curl -sSL https://get.rvm.io | bash -s stable
```

* Install Ruby 2.x. Check it by "ruby -v"
```
rvm install ruby-2.2.1
```

* Install Vagrant version **1.7.1** from this [link](https://www.vagrantup.com/download-archive/v1.7.1.html). Check it by "vagrant -v". Do not use 1.7.2 as there are known issues in version 1.7.2. Vagrant + Chef Solo + VirtualBox are **extremely** sensitive to each other!

* Install VirtualBox version 4.3.26 from this [link](https://www.virtualbox.org/wiki/Downloads)

* Install [chefdk](https://downloads.getchef.com/chef-dk/mac/#/)

* Add this line to "~/.bash_profile" and then run "source ~/.bash_profile"
```
PATH='/opt/chefdk/bin:'$PATH
```

* Make sure your ssh keys work with GitHub. Check it by "ssh -T git@github.com". If it doesn't work, fix it by using this [help](https://help.github.com/articles/generating-ssh-keys).

* Checkout this Git repository into a folder, e.g. ~/projects/instant-coder

* Run 'bundle', e.g.
```
cd ~/projects/instant-coder
bundle install
```

* Run 'librarian-chef', e.g.
```
cd ~/projects/instant-coder
librarian-chef install
```

* Now you are ready to bring up the virtual machine. It can take up to 30 to 60 minutes to finish. Please let the process finish and do not attempt to login before it finishes. If it fails in the middle of it for any reason, e.g. some gem repoistory is unreachable, run it again by running "vagrant reload --provision".
```
cd ~/projects/instant-coder
vagrant up
```

* Other useful vagrant commands:
```
vagrant up
vagrant halt
vagrant suspend
vagrant reload
vagrant destroy
```

* After the "vagrant up" finishes successfully (hopefully it does!), you have these software installed and you are almost ready to code!
  1. sudo-able user 'dev' with password 'password'. Its SSH keys are copied from your Mac as well.
  2. build-essential for compiling native libraries
  3. ntp
  4. git
  5. vim
  6. Imagemagick (required by Paperclip gem)
  7. redis (required by Resque gem)
  8. Java
  9. MySQL with root's password 'mysqlforyou'
  10. Git-friendly prompt (it shows you which branch or tag you are at when you are inside a git-managed folder)
  11. rbenv (a more light-weighted replacement for RVM. Check [this](https://github.com/sstephenson/rbenv) out!) with Ruby 2.2.1. Bundler and Rails gem are also installed.
  12. Sublime Text 3 (launch it by 'sublime' command)

## Launching your first Rails project
  1. Login to the VM using 'dev' user and password 'password'.
  2. mkdir -p projects
  3. cd projects
  4. rails new hellworld --database mysql
  5. Open sublime and edit ~/projects/helloworld/config/database.yml to change the database password to 'mysqlforyou'
  6. cd helloworld
  7. rake db:create
  8. rails s
  9. Open http://localhost:3000. Here you go!
  10. If you want to access the Rails server from your Mac, just use http://192.168.50.10:3000
