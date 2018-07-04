# frozen_string_literal: true

source "https://rubygems.org"
gemspec

gem "jekyll", "~> 3.6.0"
gem 'pygments.rb'
gem 'rdiscount', '~> 2.1.7.1'
gem 'jekyll-seo-tag'

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.6"
  gem "jekyll-paginate", "~> 1.1.0"
end

require 'rbconfig'
  if RbConfig::CONFIG['target_os'] =~ /darwin(1[0-3])/i
    gem 'rb-fsevent', '<= 0.9.4'
  end
