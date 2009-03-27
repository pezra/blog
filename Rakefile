require 'rake/clean'
require 'open3'

FileList["*/text.md"].each do |a_post|
  post_name = File.dirname(a_post)

  desc "Generate HTML fragment from post text"
  file "#{post_name}/text.html_frag" => "#{post_name}/text.md" do 
    Dir.chdir(post_name) do 
      `maruku --html-frag text.md`
      `sed --in-place 's/\\(fn\\(ref\\)\\?:\\)/\\0#{post_name}-/g' text.html_frag`    
    end
  end

  namespace post_name do 
    desc "Generate HTML fragment from post text and copy it to clipboard"
    task :copy => "#{post_name}/text.html_frag" do 
      `xsel --clipboard < #{post_name}/text.html_frag`
    end
  end
end

CLEAN.include(FileList['*/text.html_frag'])