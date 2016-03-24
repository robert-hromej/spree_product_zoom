require 'rake'
require 'rake/testtask'
require 'rake/packagetask'
require 'rubygems/package_task'
require 'rspec/core/rake_task'
require 'spree/testing_support/common_rake'

RSpec::Core::RakeTask.new

task :default => [:spec]

spec = eval(File.read('spree_product_zoom.gemspec'))

Gem::PackageTask.new(spec) do |p|
  p.gem_spec = spec
end

desc 'Release to gemcutter'
task :release => :package do
  require 'rake/gemcutter'
  Rake::Gemcutter::Tasks.new(spec).define
  Rake::Task['gem:push'].invoke
end

desc 'Generates a dummy app for testing'
task :test_app do
  ENV['LIB_NAME'] = 'spree_product_zoom'
  images_path = File.join(FileUtils.pwd, 'spec', 'products_images', '.')
  dummy_path = File.join(FileUtils.pwd, 'spec', 'dummy', 'public', 'spree', 'products')
  Rake::Task['common:test_app'].invoke

  puts "Copying product images..."
  FileUtils.mkdir_p(dummy_path)
  FileUtils.cp_r(images_path, dummy_path)

  puts "Precompiling assets (again)..."
  system("bundle exec rake assets:precompile > #{File::NULL}")

end
