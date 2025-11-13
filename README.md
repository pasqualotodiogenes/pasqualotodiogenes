```ruby

module TextFormatter
  def format_key(key)
    key.to_s.gsub('_', ' ').split.map(&:capitalize).join(' ')
  end
end

# The Skills class is entirely dynamic. It renders any data structure it receives without needing to know the categories beforehand.

class Skills
  include TextFormatter

  def initialize(skills_data)
    @data = skills_data || {}
  end

  def to_s
    output = ["### Technical Skills\n"]

    # Iterates over each main skill category 
    @data.each do |category, sub_skills|
      category_name = format_key(category)
      output << "**#{category_name}:**"

      # If the category has subcategories 
      if sub_skills.is_a?(Hash)
        sub_skills.each do |sub_category, items|
          sub_category_name = format_key(sub_category)
          output << "- **#{sub_category_name}:** #{items.join(', ')}"
        end
      # If it's just a simple list of items
      elsif sub_skills.is_a?(Array)
        output << "- #{sub_skills.join(', ')}"
      end
      output << "" # Adds a blank line for spacing
    end

    output.join("\n").strip
  end
end

class DeveloperProfile
  include TextFormatter
  attr_reader :data, :skills

  def initialize(data)
    @data = data
    @skills = Skills.new(data[:skills] || {})
  end

  def to_s
    profile_parts = []
    profile_parts << "# #{data[:name]}"
    profile_parts << "\n*#{data[:area]}* | *#{data[:job]}* | *#{data[:location]}*\n"

    if data[:about]
      profile_parts << "### About Me"
      profile_parts << data[:about]
      profile_parts << ""
    end

    profile_parts << "---"
    profile_parts << skills.to_s
    profile_parts << "---"

    if data[:contact]
      profile_parts << "\n### Contact\n"
      contact_links = data[:contact].map do |platform, url|
        "[#{format_key(platform)}](#{url})"
      end
      profile_parts << contact_links.join(' | ')
    end

    profile_parts.join("\n")
  end
end

# --- Main Script Logic ---

PROFILE_DATA = {
  name: "Diógenes da Silva Pasqualoto",
  area: "Software Development",
  job: "Seeking new opportunities",
  location: "Rio Grande do Sul, Brazil",
  about: "A developer with a focus on solid engineering fundamentals, experienced in Ruby and JavaScript. I'm familiar with technologies like Vue.js and React and have the ability to learn and adapt to new tools as needed.",
  contact: {
    linkedin: "https://www.linkedin.com/in/diogenes-pasqualoto-203750331/",
    github: "https://github.com/pasqualotodiogenes",
    email: "mailto:diogenespasqualoto147@gmail.com"
  },
  skills: {
    front_end: {
      fundamentals: ["HTML", "CSS", "JavaScript (ES6+)"],
      frameworks: ["Vue.js", "React"],
      libraries: ["jQuery", "Bootstrap", "TailwindCSS"],
      tools: ["Webpack", "Gulp", "Vite"]
    },
    back_end: {
      languages: ["JavaScript", "Ruby"],
      frameworks: ["Node.js", "Express", "Rails"],
      databases: ["SQLITE", "MySQL", "MongoDB", "PostgreSQL", "Redis"]
    },
    dev_ops: {
      tools: ["Docker", "Git", "Linux"]
    },
    methodologies: ["Scrum", "Kanban", "TDD"]
  }
}

# --- Execution ---

developer = DeveloperProfile.new(PROFILE_DATA)
output = developer.to_s
if ARGV.first == '--write'
  File.write('README.md', output)
  puts "✅ Profile saved successfully to 'README.md'!"
else
  # If no arguments are passed, just print to the console.
  puts output
end

```
