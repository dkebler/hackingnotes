### Chef


11/17/14
check on the latest chef version here
http://downloads.getchef.com/chef-dk/ubuntu/#/

Then use these commands
```
wget https://opscode-omnibus-packages.s3.amazonaws.com/ubuntu/12.04/x86_64/chefdk_0.3.5-1_amd64.deb
sudo dpkg -i chefdk_0.3.5-1_amd64.deb
```

This loads a chef ruby install but it's not in the path.

after getting the dk loaded it comes with ruby so best to put it in the path
$export PATH="/opt/chefdk/embedded/bin:${HOME}/.chefdk/gem/ruby/2.1.0/bin:$PATH"

You can do that per session or add it to the .profile file using nano in th ubuntu account.

Here is a good blog on getting started
http://jtimberman.housepub.org/blog/2014/04/30/chefdk-and-ruby/