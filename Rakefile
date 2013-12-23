#!/bin/env ruby
# encoding: utf-8

# require 'rubygems'
require 'rake'
# require 'rdoc'
# require 'date'
# require 'yaml'
# require 'tmpdir'
require 'jekyll'
require 'git'
# require 'logger'

# desc "Generate blog files"
# task :generate do
#   Jekyll::Site.new(Jekyll.configuration({
#     "source"      => ".",
#     "destination" => "_site"
#   })).process
# end


desc "Generate and publish blog to master"
task :publish do
    system "bundle exec jekyll build"

     gs = Git.open( File.dirname(__FILE__))
     gs.status.count

     if gs.status.count then
     	puts " "
     	print "Existe(m) "+gs.status.count.to_s+" arquivos não comitados.\nTem certeza que deseja continuar? [s/N]:"
     	continuar = $stdin.gets.chomp
     	if (continuar != "s" && continuar != "S") then
     		puts "Execução cancelada"
     		puts " "
     		next
     	end
     end

    last_commit = gs.log.first
    message = "Publish on "+ DateTime.now.strftime("%y-%m-%d %H:%m:%S") +" of commit " + last_commit.sha + " from " + last_commit.date.strftime("%y-%m-%d %H:%m:%S")
    if gs.status.count then
    	message = message + " with dirty files"
    end

    # p commit
    # p commit.message


    dist_dir = Jekyll.configuration({})['destination']
    dist_path = File.absolute_path(dist_dir)

    gd =  Git.open( dist_path )
    gd.add(:all=>true)  
    gd.commit_all(message)
    
    # Needs ssh key... was not working with ruby
    system "git push origin master --force"

    puts "Publish finished. :)"

    # p gd.status

    # Dir.chdir( dist_dir ) do
    #   system "pwd"
    #   p "#{message.shellescape}"
    # end

# task :publish => [:generate] do
    
  # Dir.mktmpdir do |tmp|
  #   system "bundle exec jekyll build"
  #   system "git checkout -b gh-pages"
  #   system "rm -rf *"
  #   system "mv #{tmp}/* ."
  #   message = "Site updated at #{Time.now.utc}"
  #   system "git add ."
  #   system "git commit -am #{message.shellescape}"
  #   system "git push origin gh-pages --force"
  #   system "git checkout master"
  #   system "echo yolo"
  # end
end

task :default => :publish