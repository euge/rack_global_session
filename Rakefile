#--  -*-ruby-*-
# Copyright: Copyright (c) 2010 RightScale, Inc.
#
# Permission is hereby granted, free of charge, to any person obtaining
# a copy of this software and associated documentation files (the
# 'Software'), to deal in the Software without restriction, including
# without limitation the rights to use, copy, modify, merge, publish,
# distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so, subject to
# the following conditions:
#
# The above copyright notice and this permission notice shall be
# included in all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
# IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
# CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
# TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
#++

require 'rubygems'
require 'fileutils'
require 'rake'
require 'spec/rake/spectask'
require 'rake/rdoctask'
require 'rake/gempackagetask'
require 'rake/clean'

task :default => 'spec'

load

Rake::GemPackageTask.new(Gem::Specification.load('rack_global_session.gemspec')) do |package|
  package.need_zip = true
  package.need_tar = true
end

CLEAN.include('pkg')

# == Unit Tests == #

desc "Run unit tests"
Spec::Rake::SpecTask.new do |t|
  t.spec_files = Dir['spec/**/*_spec.rb']
  t.spec_opts = lambda do
    IO.readlines(File.join(File.dirname(__FILE__), 'spec', 'spec.opts')).map {|l| l.chomp.split " "}.flatten
  end
end

desc "Run unit tests with RCov"
Spec::Rake::SpecTask.new(:rcov) do |t|
  t.spec_files = Dir['spec/**/*_spec.rb']
  t.rcov = true
  t.rcov_opts = lambda do
    IO.readlines(File.join(File.dirname(__FILE__), 'spec', 'rcov.opts')).map {|l| l.chomp.split " "}.flatten
  end
end

desc "Print Specdoc for unit tests"
Spec::Rake::SpecTask.new(:doc) do |t|
   t.spec_opts = ["--format", "specdoc", "--dry-run"]
   t.spec_files = Dir['spec/**/*_spec.rb']
end

# == Documentation == #

desc "Generate API documentation to doc/rdocs/index.html"
Rake::RDocTask.new do |rd|
  rd.rdoc_dir = 'doc/rdocs'
  rd.main = 'README.rdoc'
  rd.rdoc_files.include 'README.rdoc', "lib/**/*.rb"
end

# == Emacs integration == #
desc "Rebuild TAGS file"
task :tags do
  sh "rtags -R lib spec"
end
