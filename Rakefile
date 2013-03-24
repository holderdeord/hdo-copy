require 'rake/clean'
require 'redcarpet'

directory 'build'
CLEAN << 'build'

module Hdo
  class MarkdownRenderer < Redcarpet::Render::HTML
    def header(str, level)
      id = str.gsub(/\s+/, '-').gsub(/[^\w-]/, '').downcase
      "<h#{level} id='#{id}'>#{str}</h#{level}>"
    end
  end
end

task :default => :clean do
  renderer = Hdo::MarkdownRenderer.new(with_toc_data: true)
  markdown = Redcarpet::Markdown.new(renderer,
    autolink: true,
    space_after_headers: true,
    tables: true,
    fenced_code_blocks: true
  )

  FileList['pages/**/*.md'].each do |source|
    dest = source.gsub(/pages\/(.+)\.md/, 'build/\1.html')
    mkdir_p File.dirname(dest), verbose: false

    open(dest, "w") { |file| file << markdown.render(File.read(source)) }

    puts "#{source} => #{dest}"
  end
end