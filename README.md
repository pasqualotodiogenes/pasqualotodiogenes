```ruby
module TextFormatter
  def format_key(key)
    key.to_s.gsub('_', ' ').split.map(&:capitalize).join(' ')
  end
end

# A classe Skills é totalmente dinâmica. Ela renderiza qualquer estrutura de dados que receber, sem conhecer as categorias de antemão.

class Skills
  include TextFormatter

  def initialize(skills_data)
    @data = skills_data || {}
  end

  def to_s
    output = ["### Habilidades Técnicas\n"]

    # Itera sobre cada categoria principal de habilidade (ex: :front_end)
    @data.each do |category, sub_skills|
      category_name = format_key(category)
      output << "**#{category_name}:**"

      # Se a categoria tiver subcategorias (como :linguagens, :frameworks)
      if sub_skills.is_a?(Hash)
        sub_skills.each do |sub_category, items|
          sub_category_name = format_key(sub_category)
          output << "- **#{sub_category_name}:** #{items.join(', ')}"
        end
      # Se for apenas uma lista simples de itens
      elsif sub_skills.is_a?(Array)
        output << "- #{sub_skills.join(', ')}"
      end
      output << "" # Adiciona uma linha em branco para espaçamento
    end

    output.join("\n").strip
  end
end



class Desenvolvedor
  include TextFormatter
  attr_reader :data, :skills

  def initialize(data)
    @data = data
    @skills = Skills.new(data[:skills] || {})
  end

  def to_s
    profile_parts = []
    profile_parts << "# #{data[:nome]}"
    profile_parts << "\n*#{data[:area]}* | *#{data[:trabalho]}* | *#{data[:local]}*\n"

    if data[:sobre]
      profile_parts << "### Sobre Mim"
      profile_parts << data[:sobre]
      profile_parts << ""
    end

    profile_parts << "---"
    profile_parts << skills.to_s
    profile_parts << "---"

    if data[:contato]
      profile_parts << "\n### Contato\n"
      contact_links = data[:contato].map do |plataforma, url|
        "[#{format_key(plataforma)}](#{url})"
      end
      profile_parts << contact_links.join(' | ')
    end

    profile_parts.join("\n")
  end
end

# --- Lógica Principal do Script ---

DADOS_DO_PERFIL = {
  nome: "Diógenes da Silva Pasqualoto",
  area: "Desenvolvimento de Software",
  trabalho: "Buscando novas oportunidades",
  local: "Rio Grande do Sul, Brasil",
  sobre: "Desenvolvedor apaixonado por criar soluções eficientes e elegantes, com foco em Ruby e JavaScript. Sempre buscando aprender novas tecnologias e aprimorar minhas habilidades.",
  contato: {
    linkedin: "https://www.linkedin.com/in/seu-usuario/",
    github: "https://github.com/pasqualotodiogenes",
    email: "mailto:seuemail@example.com"
  },
  skills: {
    front_end: {
      linguagens: ["HTML", "CSS", "JavaScript", "TypeScript"],
      frameworks: ["Vue.js", "React"],
      bibliotecas: ["jQuery", "Bootstrap", "TailwindCSS"],
      ferramentas: ["Webpack", "Gulp", "Vite"]
    },
    back_end: {
      linguagens: ["JavaScript", "Ruby"],
      frameworks: ["Node.js", "Express", "Rails"],
      bancos_de_dados: ["SQLITE", "MySQL", "MongoDB", "PostgreSQL", "Redis"]
    },
    dev_ops: {
      ferramentas: ["Docker", "Git", "Linux"]
    },
    metodologias: ["Scrum", "Kanban", "TDD"]
  }
}

# --- Execução ---

desenvolvedor = Desenvolvedor.new(DADOS_DO_PERFIL)
output = desenvolvedor.to_s
if ARGV.first == '--write'
  File.write('README.md', output)
  puts "✅ Perfil salvo com sucesso em 'README.md'!"
else
  # Se nenhum argumento for passado, apenas imprime no console.
  puts output
end

```
