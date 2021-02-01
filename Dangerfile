# frozen_string_literal: true

# A Danger script which checks out dependent projects and builds them against the
# current (installed) version of this project.
#
# XXX: This script can be improved further. Open points:
# - The script assumes that the parent project resides in the current directory
#   and has already been installed into the local Maven repository.
# - The script assumes this project and all downstream dependencies are Maven
#   projects. Think of ways to generalize this.
# - The code should be moved out of this file and molded into a proper Danger
#   plugin.

require 'nokogiri'
require 'yaml'

class Project
  attr_reader :directory, :group_id, :artifact_id, :version

  def self.load(directory)
    File.open("#{directory}/pom.xml") do |f|
      project = Nokogiri.XML(f).xpath('/xmlns:project')
      parent = project.xpath('xmlns:parent')

      first_present = lambda do |expr|
        [project, parent].lazy.flat_map { |n| n.xpath(expr) }.detect(&:itself)
      end

      Project.new(
        directory,
        first_present.call('xmlns:groupId/text()'),
        project.xpath('xmlns:artifactId/text()'),
        first_present.call('xmlns:version/text()')
      )
    end
  end

  def initialize(directory, group_id, artifact_id, version)
    @directory = directory
    @group_id = group_id
    @artifact_id = artifact_id
    @version = version
  end

  def execute(command)
    system("cd #{@directory} && #{command}")
  end

  def upgrade_to(other)
    other.upgrade_files_in(@directory)
    execute("echo 'Upgraded #{self} to #{other}:' && git diff")
  end

  def upgrade_files_in(dependent_project_dir)
    Dir.glob("#{dependent_project_dir}/**/pom.xml").each do |pom_file|
      pom = File.open(pom_file) { |f| Nokogiri.XML(f) }

      version_references =
        pom.xpath(
          '//xmlns:*[xmlns:groupId = "%s" and xmlns:artifactId = "%s"]/xmlns:version' %
            [@group_id, @artifact_id]
        )
      version_references.each { |n| n.content = @version }

      File.open(pom_file, 'w') { |f| pom.write_xml_to f }
    end
  end

  def to_s
    "#{@group_id}:#{@artifact_id}:#{@version}"
  end
end

class DependantsTester
  def self.load_config(config_file)
    config = YAML.load_file(config_file)

    DependantsTester.new(
      File.expand_path(
        File.dirname(config_file),
        config.fetch('working_directory', '/tmp')
      ),
      config['dependants']
    )
  end

  def initialize(workdir, dependants)
    @workdir = workdir
    @dependants = dependants
  end

  def try_upgrades_to(project)
    warnings = []
    @dependants.each do |dependent|
      repo = dependent['repo']
      build = dependent['build']

      downstream = Project.load(git_fetch_or_update(repo))
      downstream.upgrade_to(project)
      if !downstream.execute(build)
        warnings <<
          <<~EOF
          It appears that this change breaks `#{downstream}`.

          Test command used:
          ```sh
          #{build}
          ```
          Environment:
          ```
          #{`mvn -version`.strip}
          ```
          Check the build log for details.
        EOF
      end
      git_reset(downstream.directory)
    end
    warnings
  end

  private

  def git_fetch_or_update(git_url)
    directory = "#{@workdir}/#{git_url.gsub(%r{[:\/.]}, '__')}"

    if !Dir.exist?(directory)
      system("git clone --depth 1 '#{git_url}' '#{directory}'") or exit
    end
    git_reset(directory)

    directory
  end

  def git_reset(dir)
    git = "git -C '#{dir}'"
    system("#{git} clean -fdx") or exit
    system("#{git} clean -fdX") or exit
    system("#{git} checkout -- .") or exit
    system("#{git} submodule update --depth 1 --init --recursive") or exit

    foreach_submodule = "#{git} submodule foreach --recursive"
    system("#{foreach_submodule} 'git clean -fdx'") or exit
    system("#{foreach_submodule} 'git clean -fdX'") or exit
    system("#{foreach_submodule} 'git checkout -- .'") or exit
  end
end

this_project = Project.load('.')
tester = DependantsTester.load_config('dependants.yml')
tester.try_upgrades_to(this_project).each { |warning| warn warning }
