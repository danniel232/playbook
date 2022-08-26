require "rake"
require "json"
require "front_matter_parser"
require "open3"

desc "Create corpus for search"
file './corpus.json' => ['./', *Rake::FileList['_guides/*.markdown', '_guides/*.md']] do |md_file|
  unsafe_loader = ->(string) { YAML.load(string) } # for front_matter_parser
  corpus = md_file.sources.grep(/\.(md|markdown)$/)
    .map do |path|
    file_path = './' + path
      parsed = FrontMatterParser::Parser.parse_file(file_path, syntax_parser: :md, loader: unsafe_loader)
      {
        id: path.pathmap('%n'),
        name: parsed['title'],
        content: parsed.content,
      }
    end

  File.open(md_file.name, 'w') do |f|
    f << JSON.generate(corpus)
  end
end

task :default => ['./corpus.json']
