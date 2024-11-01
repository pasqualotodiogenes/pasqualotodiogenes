```ruby
class Desenvolvedor
  attr_accessor :nome, :area, :trabalho, :local

  def initialize(nome, area, trabalho, local)
    @nome = nome
    @area = area
    @trabalho = trabalho
    @local = local
  end
end

class SobreMim < Desenvolvedor
  def initialize
    super('Diógenes da Silva Pasqualoto', 'TI', 'for now looking...', 'RS')
  end

  def to_s
    "Nome: #{@nome}\nÁrea: #{@area}\nTrabalho: #{@trabalho}\nLocal: #{@local}"
  end
end

class Skills < Desenvolvedor
  def initialize
    super('Diógenes da Silva Pasqualoto', 'TI', 'for now looking...', 'RS')

    @front_end = {
      linguagens: ['HTML', 'CSS', 'JavaScript'],
      frameworks: ['Vue.js'],
      bibliotecas: ['jQuery', 'Bootstrap'],
      ferramentas: ['Webpack', 'Gulp']
    }

    @back_end = {
      linguagens: ['JavaScript', 'Ruby'],
      frameworks: ['Node.js', 'Express', "Rails"],
      bancos_de_dados: ['SQLITE', 'MySQL', 'MongoDB', 'PostgreSQL']
    }

    @outras_habilidades = ['Git', 'Linux']

    @metodologias = ['Scrum', 'Kanban']
  end

  def to_s
    skills = "Front-End:\n  Linguagens: #{@front_end[:linguagens]}\n  Frameworks: #{@front_end[:frameworks]}\n  Bibliotecas: #{@front_end[:bibliotecas]}\n  Ferramentas: #{@front_end[:ferramentas]}"
    skills += "\nBack-End:\n  Linguagens: #{@back_end[:linguagens]}\n  Frameworks: #{@back_end[:frameworks]}\n  Bancos de Dados: #{@back_end[:bancos_de_dados]}"
    skills += "\nOutras Habilidades:\n  #{@outras_habilidades}"
    skills += "\nMetodologias:\n  #{@metodologias}"

    super + "\n#{skills}"
  end
end

sobre_mim = SobreMim.new
puts sobre_mim

skills = Skills.new
puts skills

```
