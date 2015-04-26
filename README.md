# What is instant coder?
**instant coder** is a very simple wrapper of Vagrant and Chef Solo. Doing a lot of RoR development on Mac (or even Windows?) is great, but when it comes to a more serious applicaiton to be deployed on AWS or other cloud provides it's better to have an identical environment all the way from development to production. **instant coder** is here to let you, the coder, start with your RoR development enviornment on Ubuntu (xubuntu in this project as it is more lightweight) using VirtualBox, Vagrant, and Chef Solo technologies.

## IMPORTANT: Before you proceed, please make sure...
* You have a stable and fast internet connection.
* You have 20 to 30 minutes for Chef to finish the entire process, which involves downloading and compiling various components of the software stack.
* You haven't set a passphrase for your SSH key in your MacOS because the entire process is CLI-based and it does not bring up a GUI to ask you for the password. You can check it by following this <a href=http://unix.stackexchange.com/questions/120424/is-there-a-way-to-check-a-users-ssh-key-to-see-if-the-passphrase-is-blank target=_blank>instruction</a>. You can remove the passphrase by "ssh-keygen -p" or read this <a href=http://stackoverflow.com/questions/112396/how-do-i-remove-the-passphrase-for-the-ssh-key-without-having-to-create-a-new-ke target=_blank>instruction</a> for details.

## IMPORTANT: Retina user only
Some of you may experience sluggish display performance issue in the guest operating system when using VirtualBox under retina display. The symptom is some display lag when you do typing or scrolling in the system. You can work around it by disabling hiDPI on your Mac but having hiDPI is the whole point of getting a retina display. VMware has a better support of retina display and there are other benefits like better default keyboard mapping (yes, command-c and command-v to do copy & paste in Ubuntu!), distraction free full screen mode, lower overhead and faster performance...etc. So I recommend buying VMware Fusion and Vagrant for VMware in this case.

To use **instant coder** for VMware, it is pretty simple:

1. Buy and install VMware Fusion.
2. Buy and install Vagrant for VMware
3. Use Vagrantfile.vmware instead of Vagrantfile and the rest of the steps in this README are the same, i.e.
```
cp Vagrantfile.vmware Vagrantfile
```

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

* Install Vagrant version **1.7.1** from this <a href=https://www.vagrantup.com/download-archive/v1.7.1.html target=_blank>link</a>. Check it by "vagrant -v". **Do not use 1.7.2** as there are known issues in version 1.7.2. Vagrant + Chef Solo + VirtualBox are **extremely** sensitive to each other!

* Install VirtualBox version 4.3.26 from this <a href=https://www.virtualbox.org/wiki/Downloads target=_blank>link</a>

* Install <a href=https://downloads.getchef.com/chef-dk/mac/#/ target=_blank>chefdk</a>

* Add this line to "~/.bash_profile" and then run "source ~/.bash_profile"
```
PATH='/opt/chefdk/bin:'$PATH
```

* Make sure your ssh keys work with GitHub. Check it by "ssh -T git@github.com". If it doesn't work, fix it by using this <a href=https://help.github.com/articles/generating-ssh-keys target=_blank>help</a>

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

* Now you're ready to bring up the virtual machine. It can take 30 to 60 minutes to finish. Please let the process finish and **do not attempt to login before it finishes**. If it fails in the middle of it for any reason, e.g. some gem repoistory is unreachable, run it again by running "vagrant reload --provision".
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

* After the "vagrant up" finishes successfully (hopefully it does!), you'll have the following software installed and you're almost ready to code!
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
