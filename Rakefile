require 'date'

task :draft do
  title = ENV['title']
  system %Q[touch "#{title}.md"]
end

task :publish do
  old_file_name = ENV['article']
  
  date_string = Date.today.strftime("%Y-%m-%d")
  *path, file_name = old_file_name.split('/')
  new_file_name = %Q[#{path.join('/')}#{path.empty? ? "" : "/"}#{date_string} #{file_name}]
  
  system %Q[git mv "#{old_file_name}" "#{new_file_name}"]
  system %Q[git commit -m "Publish #{file_name}"]
end