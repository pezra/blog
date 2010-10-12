require 'rake/clean'
require 'open3'
require 'maruku'

FileList["*.md"].each do |a_post|
  post_name = File.basename(a_post, '.md')

  desc "Generate HTML fragment from post text"
  file "#{post_name}.html_frag" => "#{post_name}.md" do 
    d = Maruku.new(File.read("#{post_name}.md"), :doc_prefix => post_name)

    File.open("#{post_name}.html_frag", 'w'){|f| f.write(d.to_html)}
  end


  namespace post_name do 
    desc "Generate HTML fragment from post text and copy it to clipboard"
    task :copy => "#{post_name}.html_frag" do
      File.popen("pbcopy", "w") do |xsel|
        xsel.write(post)
      end
    end

    desc "upload images"
    task :upload_images do 
      system "ssh barelyen@barelyenough.org 'mkdir -p public_html/wordpress/wp-content/uploads/#{post_name}/'"
      system "scp #{post_name}/*.png barelyen@barelyenough.org:public_html/wordpress/wp-content/uploads/#{post_name}/"
      system "scp #{post_name}/*.jpg barelyen@barelyenough.org:public_html/wordpress/wp-content/uploads/#{post_name}/"
    end
  end
end

FileList["*/text.md"].each do |a_post|
  post_name = File.dirname(a_post)

  desc "Generate HTML fragment from post text"
  file "#{post_name}/text.html_frag" => "#{post_name}/text.md" do 
    Dir.chdir(post_name) do 
      d = Maruku.new(File.read("text.md"), :doc_prefix => post_name)

      File.open("text.html_frag", 'w'){|f| f.write(d.to_html)}
    end
  end

  namespace post_name do 
    desc "Generate HTML fragment from post text and copy it to clipboard"
    task :copy => "#{post_name}/text.html_frag" do
      File.popen("pbcopy", "w") do |xsel|
        xsel.write(post)
      end
    end
  end

end

CLEAN.include(FileList['*/text.html_frag'], FileList['*.html_frag'])
