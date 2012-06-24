require 'date'

desc "Create a new file with title as file name"
task :draft do
  title = ENV['title']
  system %Q[touch "draft/#{title}.md"]
end

desc "Add date stamp to file name so that it will be picked up as published"
task :publish do
  old_file_name = ENV['article']
  
  if old_file_name =~ /\Adraft\/(.+)(\.md)$/
    date = Date.today.strftime("%Y-%m-%d")

    new_file_name = "published/#{date} #{$1}#{$2}"

    system 'git mv "#{old_file_name}" "#{new_file_name}"'
    system %(git commit -m "Published #{$1}")
  else
    raise "That's not an article!"
  end
end