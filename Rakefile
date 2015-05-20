task :default => 'tasklist'
desc "List & Describe Tasks"
task :tasklist do
  sh "rake -P"
  sh "rake -T"
end

namespace :composer do

  composer_phar="./composer.phar"
  composer_lock="./composer.lock"
 
  desc "download composer.phar executable - if not exist"
  task :download do
    sh "curl -s https://getcomposer.org/installer | php" unless File.exist?("#{composer_phar}")
  end

  desc "remove the composer.phar executable"
  task :remove do
    sh "rm #{composer_phar}"
  end

  desc "update to lastest version of composer.phar executable"
  task :selfupdate =>['composer:download'] do
    sh "#{composer_phar} self-update"
  end

  desc "Install composer dependencies"
  task :install =>['composer:selfupdate'] do
    sh "#{composer_phar} install"
  end

  desc "Update composer dependencies"
  task :update =>['composer:selfupdate'] do
    sh "#{composer_phar} update"
  end

  desc "Install/Update composer dependencies"
  task :build do
     puts "Install/Update composer dependencies ..."
     if File.exist?("#{composer_lock}")
       Rake::Task["composer:update"].invoke
     else
       Rake::Task["composer:install"].invoke
     end
  end

  desc "Remove composer build"
  task :clean do
    puts 'cleaning up (composer)...'
    sh 'rm -rf vendor'
    sh 'rm composer.lock'
  end

end




# project:build
namespace :project do
  desc "Build Project"
  task :build=>['composer:build']
end

# project:clean
namespace :project do
  desc "Clean Project"
  task :clean=>['composer:clean']
end

# project:test
namespace :project do

  ##phpunit = "phpunit --configuration ./phpunit.xml"

  desc "Build & Test"
  task :test =>['project:build'] do
    # depends on build task
    puts "Running unit tests ..."
    sh './vendor/bin/phpunit test'  
  end	
end




