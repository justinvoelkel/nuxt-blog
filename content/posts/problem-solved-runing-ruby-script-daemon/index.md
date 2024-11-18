---
title: Problem Solved - Running a Ruby Script as a Daemon
date: 2013-11-20T22:30:07.000Z
description: Problem Solved - Running a Ruby Script as a Daemon
---

In my last post about the [Laravel and CakePHP](http://justinvoelkel.me/wonderful-world-php-frameworks-cakephp-laravel/ 'The Wonderful World of PHP Frameworks: CakePHP and Laravel') frameworks, I mentioned briefly I was contemplating trying RoR (Ruby on Rails) – and that I had begun picking up some Ruby as a result. As luck would have it I received an email from someone I had previously done work for asking for help on a project that would involve a small Ruby script. No better time than now to jump in, right?

## The Problem

We needed a script to parse and save JSON data streamed from the Cisco precision API. Cisco offered a small sample of ruby code to get started and it worked really well. From there I  just had to send those values to a MySQL database.  With ruby installed and four gems – Sinatra, MySQL, JSON, and Daemons – that was all a snap. That last one is why I wrote this post. The script ended up looking like the following:

    require 'rubygems' require 'sinatra' require 'json' require 'mysql' if ARGV.size < 2 # The sinatra gem parses the -o and -p options for us. puts "usage: push-server.rb -o <domain.com> -p <port> <secret> <validation>" exit 1 end PORT      = ARGV[-3] SECRET    = ARGV[-2] VALIDATOR = ARGV[-1] get '/events' do VALIDATOR end post '/events' do map = JSON.parse(params[:data]) if map['secret'] != SECRET logger.warn "got post with bad secret: #{SECRET}" return end con = Mysql.new('localhost','user','pass','database') map['probing'].each do |c| con.query("INSERT INTO database (port, ap_mac, device_mac, timestamp, rssi ) VALUES('#{PORT}','#{c['ap_mac']}','#{c['client_mac']}','#{c['last_seen']}','#{c['rssi']}')"); end con.close "" end

With the script working and actively sending data to the database I realized that I had a problem. The script was only going to run while I was logged into a terminal session – should that connection end so would the script. Having never done this before, making that happen sounded like it would take some sort of wizardry. Not so much.

## The Solution

<div class="wp-caption alignright" id="attachment_152" style="width: 197px">![daemon not damon](http://justinvoelkel.me/wp-content/uploads/2013/11/matt-damon-234x300.jpg)

</div>A quick google search revealed a consensus answer – the daemons gem. If your familiar with gems then you know installation is super easy:

# gem install daemons

You can check the [documentation here](http://daemons.rubyforge.org/Daemons.html 'Ruby daemons gem documentation'). There are several ways in which to use daemons listed in that documentation. It took some trial and error for me to figure out what fit my needs – you know ‘cuz I’ve not done this before. So, I started out with run. Turns out this will only run your script once unless you provide it a loop. Well, the problem there is that a loop implies a finite number of iterations. This script was meant to run indefinitely – not gonna  work. In fact most of these options require you to provide the daemon a loop. Except for run_proc. It allows you to provide the same options as run, which was really important because it allows you to define things like app name, monitoring, and multiple instances of your process.  But, instead of requiring you to loop the source you can define an exec command for your script and be done with it. Check out the example below:

    require 'rubygems' require 'daemons' pwd = File.dirname(File.expand_path(__FILE__)) # the current directory file = pwd + '/push-server.rb' options = { :app_name    =>"meraki_push_server", :multiple   => true, :mode       => :load, :log_output => true, :monitor    => true } DOMAIN        = ARGV[-4] PORT        = ARGV[-3] SECRET         = ARGV[-2] VALIDATOR     = ARGV[-1] Daemons.run_proc('meraki_push_server',options) do exec "ruby #{file} -o #{DOMAIN} -p #{PORT} #{SECRET} #{VALIDATOR}" end

So, now I just need to provide my script the correct arguments and let it go AND on top of that I can run multiple instances of it super quickly and efficiently? Sign me up, bro.

## Conclusion

It wasn’t a rails project or anything of too much substance but my first toe dip into ruby end of the pool was at least a successful learning experience. It’s worth a note that – I believe I read – the daemon gem is unstable or doesn’t play nice with windows systems so is preferably used with a LAMP stack.

This just goes to show that even the seemingly most complicated tasks can be tackled more simply than initially thought. Especially as things like ruby gems and [RVM](https://rvm.io/ 'Ruby Version Manager') get built out to make our lives as developers (and uber n00bs like me) simpler.
