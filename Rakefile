require 'rake/clean'
require 'redcarpet'

directory 'build'
CLEAN << 'build'

module Hdo
  def self.id_for(str)
    str.gsub(/\s+/, '-').gsub(/[^\w-]/, '').downcase
  end

  class DefaultRenderer < Redcarpet::Render::HTML
    def header(str, level)
      "<h#{level} id='#{Hdo.id_for str}'>#{str}</h#{level}>\n"
    end
  end

  class TocRenderer < Redcarpet::Render::HTML_TOC
    def header(str, level)
      "<li><a href='##{Hdo.id_for str}'>#{str}</a></li>\n"
    end
  end
end

task :default => :clean do
  markdown = Redcarpet::Markdown.new(Hdo::DefaultRenderer.new,
    autolink: true,
    space_after_headers: true,
    tables: true,
    fenced_code_blocks: true
  )

  toc = Redcarpet::Markdown.new(Hdo::TocRenderer.new)

  FileList['pages/**/*.md'].each do |source|
    base = source.gsub(/pages\/(.+)\.md/, 'build/\1')

    mkdir_p File.dirname(base), verbose: false

    html_path = "#{base}.html"
    toc_path = "#{base}.toc.html"

    open(html_path, "w") { |file| file << markdown.render(File.read(source)) }
    open(toc_path, "w") { |file| file << toc.render(File.read(source)) }

    puts "#{source} => #{html_path}"
    puts "#{source} => #{toc_path}"
  end
end