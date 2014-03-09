require 'rubygems'
require 'rake'
require 'rdoc'
require 'date'
require 'yaml'
require 'tmpdir'
require 'jekyll'

desc "Generate blog files"
task :generate do
  Jekyll::Site.new(Jekyll.configuration({
    "source"      => ".",
    "destination" => "_site"
  })).process
end


desc "Generate and publish blog to gh-pages"
task :publish => [:generate] do
  Dir.mktmpdir do |tmp|
    system "mv _site/* #{tmp}"
    system "git checkout -b gh-pages"
    system "rm -rf *"
    system "mv #{tmp}/* ."
    message = "Site updated at #{Time.now.utc}"
    system "git add ."
    system "git commit -am #{message.shellescape}"
    system "git push origin gh-pages --force"
    system "git checkout master"
    system "echo yolo"
  end
end

desc "Given a title as an argument, create a new post file"
task :write, [:title, :category] do |t, args|
  filename = "#{Time.now.strftime('%Y-%m-%d')}-#{args.title.gsub(/\s/, '_').downcase}.markdown"
  path = File.join("_posts", filename)
  if File.exist? path; raise RuntimeError.new("Won't clobber #{path}"); end
  File.open(path, 'w') do |file|
    file.write <<-EOS
---
layout: post
category: #{args.category}
title: #{args.title}
date: #{Time.now.strftime('%Y-%m-%d %k:%M:%S')}
---
EOS
    end
    puts "Now open #{path} in an editor."
end

task :default => :publish
