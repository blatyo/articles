require 'date'

desc "Create a new file with title as file name"
task :draft do
  title = ENV['title']
  system %Q[touch "#{title}.md"]
end

desc "Add date stamp to file name so that it will be picked up as published"
task :publish do
  old_file_name = ENV['article']
  category = ENV['category'] ? "[#{ENV['category']}]" : ""
  tags = ENV['tags'] ? "[#{ENV['tags']}]" : ""
  
  raise "That's not an article!" if old_file_name !~ /^(?<path>.+\/)?(?<article>.+)(?<ext>\.md)$/
  
  date = Date.today.strftime("%Y-%m-%d")
  
  new_file_name = %Q|#{path}#{date}#{article}#{category}#{tags}#{ext}|
  
  system %Q[git mv "#{old_file_name}" "#{new_file_name}"]
  system %Q[git commit -m "Publish #{file_name}"]
end