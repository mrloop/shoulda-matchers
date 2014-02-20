# Adapted from: <https://github.com/norman/friendly_id/tree/gh-pages>

require 'erb'

def current_version
  @_current_version ||= Dir.chdir '.shoulda-matchers' do
    eval(File.read('lib/shoulda/matchers/version.rb'))
    Shoulda::Matchers::VERSION
  end
end

def generate_file_that_redirects_to_latest_version(version)
  erb = ERB.new(File.read('index.html.erb'))
  File.open('index.html', 'w') { |f| f.write(erb.result(binding)) }
end

def generate_docs(version, options = {})
  ref =
    if version =~ /^\d+\.\d+\.\d+/
      'v' + version
    else
      version
    end

  Dir.chdir '.shoulda-matchers' do
    sh "git clean -f"
    sh "git fetch"
    sh "git checkout #{ref}"
    sh "bundle exec yard -o ../#{ref}"
  end

  if options[:latest]
    generate_file_that_redirects_to_latest_version(ref)
  end
end

def publish_docs(version, options = {})
  message =
    if options[:latest]
      "Regenerated docs for latest version #{version}"
    else
      "Regenerated docs for version #{version}"
    end

  sh "git add -A ."
  sh "git commit -m '#{message}'"
  sh "git push"
end

file '.shoulda-matchers' do
  # FIXME: mcmire -> thoughtbot
  sh %{git clone https://github.com/mcmire/shoulda-matchers.git .shoulda-matchers}
end

namespace :docs do
  desc 'Generate docs for a particular version'
  task :generate, [:version, :is_latest] => '.shoulda-matchers' do |t, args|
    latest = (args.is_latest == 'true')
    generate_docs(args.version, latest: latest)
  end

  desc 'Generate docs for a particular version and push them to GitHub'
  task :publish, [:version, :is_latest] => '.shoulda-matchers' do |t, args|
    latest = (args.is_latest == 'true')
    generate_docs(args.version, latest: latest)
    publish_docs(args.version, latest: latest)
  end

  desc 'Generate docs for the latest version and push them to GitHub'
  task :publish_latest => '.shoulda-matchers' do
    version = current_version
    generate_docs(version, latest: true)
    publish_docs(version, latest: true)
  end
end
