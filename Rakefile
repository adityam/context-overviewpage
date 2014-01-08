require 'fileutils'
require 'rake/clean'

def make_tds(name, files, doc)
  tex_dir = "#{name}/tex/context/third/#{name}"
  doc_dir = "#{name}/doc/context/third/#{name}"
  sh "mkdir -p #{tex_dir}"
  sh "mkdir -p #{doc_dir}"
  files.each do |tex|
    FileUtils.cp tex, tex_dir
  end
  FileUtils.cp doc, (doc_dir + "/#{name}.txt")
end

def make_zip name
  # Ideally, one could have used
  # sh "zip #{name} #{name}" 
  # but that creates a top level directory #{name}.
  # So, we take the following round about alternative.
  sh "cd #{name} && zip -r ../#{name} ./ && cd ../"
end

def run_tests tests, engine
  make_pandoc
  FileUtils.mkdir_p "output"
  tests.each do |file|
    sh "context --#{engine} --color --noconsole --purgeall --purgeresult #{file}"
  end
  sh "context --purgeall"
end

TEX= %W[t-overviewpage.mkiv]
DOC  = "README.md"
TEST = FileList['tests/[0-9][0-9]-*.tex']

desc "Run tests for overview module (MkIV)"
task :test_overview => TEST do
  run_tests TEST, :luatex
end

task :clean_overview do
  sh "rm -rf overview"
end

desc "Make TDS for overview module"
task :overview => :clean_overview do
  make_tds :overview, TEX, DOC
  make_zip :overview
end

CLEAN.include('*.zip')


