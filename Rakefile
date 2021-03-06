require 'rake/testtask'
require 'rake/extensiontask'

gem_spec = eval(File.read('win32-service.gemspec'))

Rake::GemPackageTask.new(gem_spec) do |pkg|
end

Rake::ExtensionTask.new('daemon', gem_spec) do |ext|
  ext.lib_dir = 'lib/win32'
  ext.ext_dir = 'ext/win32'

  unless RUBY_PLATFORM =~ /mswin|mingw/
    ext.cross_compile = true
    ext.cross_compiling do |gemspec|
      gemspec.post_install_message = <<-MSG
Cross compiled under:
  #{RUBY_DESCRIPTION}
MSG
    end
  end
end

namespace 'gem' do
  desc 'Install the mingw binary gem'
  task :install_mingw do
    sh "gem install #{Dir['pkg/*.gem'].grep(/mingw/).join}"
  end

  desc 'Install the mswin binary gem'
  task :install_mswin do
    sh "gem install #{Dir['pkg/*.gem'].grep(/mswin/).join}"
  end
end

namespace 'test' do
  desc 'Run all tests for the win32-service library'
  Rake::TestTask.new('all') do |t|
    task :all => :compile
    t.verbose = true
    t.warning = true
  end

  desc 'Run the tests for the Win32::Daemon class'
  Rake::TestTask.new('daemon') do |t|
    task :daemon => :compile
    t.verbose = true
    t.warning = true
    t.test_files = FileList['test/test_win32_daemon.rb']
  end

  namespace 'service' do
    desc 'Run the tests for the Win32::Service class'
    Rake::TestTask.new('all') do |t|
      t.verbose = true
      t.warning = true
      t.test_files = FileList['test/test_win32_service*.rb']
    end

    Rake::TestTask.new('configure') do |t|
      t.verbose = true
      t.warning = true
      t.test_files = FileList['test/test_win32_service_configure.rb']
    end

    Rake::TestTask.new('control') do |t|
      t.verbose = true
      t.warning = true
      t.test_files = FileList['test/test_win32_service.rb']
    end

    Rake::TestTask.new('create') do |t|
      t.verbose = true
      t.warning = true
      t.test_files = FileList['test/test_win32_service_create.rb']
    end

    Rake::TestTask.new('info') do |t|
      t.verbose = true
      t.warning = true
      t.test_files = FileList['test/test_win32_service_info.rb']
    end

    Rake::TestTask.new('status') do |t|
      t.verbose = true
      t.warning = true
      t.test_files = FileList['test/test_win32_service_status.rb']
    end
  end

end

task :default => 'test:all'
