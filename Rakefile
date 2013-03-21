require 'rake/clean'
require 'redcarpet'

directory 'build'
CLEAN << 'build'

task :default => :clean do
  # TODO: find a real solution for header/anchors
  renderer = Redcarpet::Render::HTML.new(with_toc_data: true)
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