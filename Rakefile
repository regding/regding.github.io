# Refer to https://github.com/plusjade/jekyll-bootstrap/blob/master/Rakefile

require "rubygems"
require 'rake'
require 'yaml'
require 'time'

SOURCE = "."
CONFIG = {
    'posts' => File.join(SOURCE, "_posts"),
    'drafts' => File.join(SOURCE, "_drafts"),
    'post_ext' => "md",
}

# Usage: rake page name="about.html"
# You can also specify a sub-directory path.
# If you don't specify a file extention we create an index.html at the path specified
desc "Create a new page."
task :page do
    name = ENV["name"] || "new-page.md"
    filename = File.join(SOURCE, "#{name}")
    filename = File.join(filename, "index.html") if File.extname(filename) == ""
    title = File.basename(filename, File.extname(filename)).gsub(/[\W\_]/, " ").gsub(/\b\w/){$&.upcase}
    if File.exist?(filename)
        abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
    end
  
    mkdir_p File.dirname(filename)
    puts "Creating new page: #{filename}"
    open(filename, 'w') do |post|
        post.puts "---"
        post.puts "layout: page"
        post.puts "title: \"#{title}\""
        post.puts 'description: ""'
        post.puts "---"
        post.puts "{% include JB/setup %}"
    end
end # task :page

# Usage: rake post title="A Title" [date="2012-02-09"] [tags=[tag1,tag2]]
desc "Begin a new post in #{CONFIG['posts']}"
task :post do
    abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])
    title = ENV["title"] || "new-post"
    tags = ENV["tags"] || "[]"
    slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
    begin
        date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
    rescue => e
        puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
        exit -1
    end
    filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
    if File.exist?(filename)
        abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
    end

    puts "Creating new post: #{filename}"
    open(filename, 'w') do |post|
        post.puts "---"
        post.puts "layout: post"
        post.puts "title: \"#{title.gsub(/-/,' ')}\""
        post.puts 'description: ""'
        post.puts "date: #{date}"
        post.puts "tags: #{tags}"
        post.puts "comments: true"
        post.puts "---"
    end
end # task :post

# Usage: rake draft title="A Title" [date="2012-02-09"] [tags=[tag1,tag2]]
desc "Begin a new post in #{CONFIG['drafts']}"
task :draft do
    abort("rake aborted: '#{CONFIG['drafts']}' directory not found.") unless FileTest.directory?(CONFIG['drafts'])
    title = ENV["title"] || "new-draft"
    tags = ENV["tags"] || "[]"
    slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
    begin
        date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
    rescue => e
        puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
        exit -1
    end
    filename = File.join(CONFIG['drafts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
    if File.exist?(filename)
        abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
    end

    puts "Creating new drafts: #{filename}"
    open(filename, 'w') do |draft|
        draft.puts "---"
        draft.puts "layout: post"
        draft.puts "title: \"#{title.gsub(/-/,' ')}\""
        draft.puts 'description: ""'
        draft.puts "date: #{date}"
        draft.puts "tags: #{tags}"
        draft.puts "comments: true"
        draft.puts "---"
    end
end # task :draft

desc "Install Jekyll Plugins"
task :geminstall do
    system "sudo gem install jekyll-seo-tag jekyll-paginate jekyll-admin"
end # task :geminstall

desc "Launch preview environment"
task :preview do
    system "jekyll serve --incremental"
end # task :preview
