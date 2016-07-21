require 'time'
require 'yaml'

SOURCE = '.'
CONFIG = {
  'version' => "0.2.13",
  'themes' => File.join(SOURCE, "_includes", "themes"),
  'layouts' => File.join(SOURCE, "_layouts"),
  'posts' => File.join(SOURCE, "_posts"),
  'post_ext' => "md",
  'theme_package_version' => "0.1.0"
}


desc "compile and run the site"
task :default do
  pids = [
    spawn("bundle exec jekyll server -w"),
    spawn("scss --watch _assets:stylesheets"),
    spawn("coffee -b -w -o javascripts -c _assets/*.coffee")
  ]
 
  trap "INT" do
    Process.kill "INT", *pids
    exit 1
  end
 
  loop do
    sleep 1
  end
end

desc "Create a new post in #{CONFIG['posts']}"
task :post do 
  abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])

  title = ENV['title'] || 'new-post'
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
  rescue Exception => e
    puts "Error - date format must be yyyy-MM-dd, please check you typed it correctly!"
    exit -1
  end

  filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwride?", ['y', 'n']) == 'n'
  end  

  puts "Creating new post: #{filename}" 

  # write templates
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/-/, ' ')}\""
    post.puts "categories:"
    post.puts "-"
    post.puts "tags:"
    post.puts "-"
    post.puts ""
    post.puts ""
    post.puts "---"
  end
end #task :post
