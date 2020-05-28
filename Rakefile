# frozen_string_literal: true

task :serve do
  system 'bundle exec jekyll serve'
end

task :deploy do
  system 'git subtree push --prefix _site origin gh-pages'
end

namespace :build do
  task :jekyll do
    system 'env JEKYLL_ENV=production bundle exec jekyll build'
  end

  task :compress do
    $stdout.print 'Compressing site.js...'
    $stdout.flush
    Dir.glob('_site/**/*.js') do |filename|
      system "./node_modules/.bin/uglifyjs -o #{filename} #{filename}"
    end
    $stdout.puts 'done'
    $stdout.print 'Compressing *.html...'
    $stdout.flush
    system './node_modules/.bin/html-minifier --input-dir _site' \
      ' --output-dir _site --file-ext html --collapse-whitespace' \
      ' --remove-comments --remove-attribute-quotes --remove-empty-attributes' \
      ' --use-short-doctype --minify-js --minify-css'
    $stdout.puts 'done'
    $stdout.print 'Compressing *.css...'
    $stdout.flush
    Dir.glob('_site/**/*.css') do |filename|
      system "./node_modules/.bin/postcss #{filename} -o #{filename}"
    end
    $stdout.puts 'done'
    # $stdout.print 'Embedding Assets...'
    # $stdout.flush
    # system 'node _scripts/embed-assets.js `find _site -name "*.html"`'
    # $stdout.puts 'done'
  end
  task all: %i[jekyll compress]
end

task build: ['build:all']
