require 'bundler/setup'
require 'bundler/gem_tasks'
require 'rake/testtask'

Rake::TestTask.new do |t|
  t.test_files = FileList['test/*.rb']
  t.verbose = true
end

begin
  require 'rspec/core/rake_task'

  RSpec::Core::RakeTask.new(:refinement) do |task|
    task.rspec_opts = "--order rand"
    task.pattern = "spec/refinement/*_spec.rb"
  end

  RSpec::Core::RakeTask.new(:monkeypatch) do |task|
    task.rspec_opts = "--order rand"
    task.pattern = "spec/monkeypatch/*_spec.rb"
  end

  RSpec::Core::RakeTask.new(:safe) do |task|
    task.rspec_opts = "--order rand"
    task.pattern = "spec/safe/*_spec.rb"
  end

rescue LoadError
  warn "rspec unavailable"
end

task :default do
  Rake::Task["test"].invoke
  Rake::Task["monkeypatch"].invoke
  Rake::Task["safe"].invoke
  Rake::Task["refinement"].invoke if ruby_major >= 2 && ruby_minor >= 1
end

def ruby_major
  RUBY_VERSION.split(/\./)[0].to_i
end

def ruby_minor
  RUBY_VERSION.split(/\./)[1].to_i
end

# Monkey patch Bundler gem_helper so we release to our gem server instead of rubygems.org
module Bundler
  class GemHelper
    def rubygem_push(path)
      gem_server_url = 'http://gems.virginia.hoopla:9292'
      sh("gem inabox '#{path}' --host #{gem_server_url}")
      Bundler.ui.confirm "Pushed #{name} #{version} to #{gem_server_url}"
    end
  end
end
