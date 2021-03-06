#!/usr/bin/env rake
require File.expand_path('../lib/bootstrap-wysihtml5-rails/version', __FILE__)

desc "Update assets"
task 'update' do
  origin_lib_path = "bootstrap-wysihtml5/lib"
  origin_src_path = "bootstrap-wysihtml5/src"
  dest_javascript_path = "vendor/assets/javascripts/bootstrap-wysihtml5"
  dest_css_path = "vendor/assets/stylesheets/bootstrap-wysihtml5"
  
  system("rm -rf bootstrap-wysihtml5")
  system("git clone git://github.com/jhollingworth/bootstrap-wysihtml5.git")
  
  system("cp #{origin_src_path}/bootstrap-wysihtml5.css #{dest_css_path}/core.css")
  # system("cp #{origin_src_path}/bootstrap-wysihtml5.js #{dest_javascript_path}/core.js")
  
  Dir.foreach("bootstrap-wysihtml5/src/locales") do |file|
    unless file == '.' || file == '..'
      abbreviated_file_name = file.gsub('bootstrap-wysihtml5.', '')
      system("cp #{origin_src_path}/locales/#{file} #{dest_javascript_path}/locales/#{abbreviated_file_name}")
    end
  end    

  core_file = File.read("#{origin_src_path}/bootstrap-wysihtml5.js")
  original_string = /stylesheets: \[".\/lib\/css\/wysiwyg-color.css"\]/
  objective_string = 'stylesheets: ["/assets/bootstrap-wysihtml5/wysiwyg-color.css"]'
  replaced   = core_file.gsub(original_string, objective_string)

  File.open("#{dest_javascript_path}/core.js", "w") { |file| file.puts replaced }
  
  system("cp #{origin_lib_path}/js/wysihtml5-0.3.0.js #{dest_javascript_path}/wysihtml5.js")
  system("cp #{origin_lib_path}/css/wysiwyg-color.css #{dest_css_path}/wysiwyg-color.css")
  
  
  system("git status")
end

desc "Release a new version"
task "release" do    
  system("gem build bootstrap-wysihtml5-rails.gemspec")
  system("gem push bootstrap-wysihtml5-rails-#{BootstrapWysihtml5Rails::Rails::VERSION}.gem")
  system("git push")
end