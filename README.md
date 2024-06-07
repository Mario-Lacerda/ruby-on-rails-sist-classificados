# Desafio Dio - Criando um sistema de classificados com Ruby on Rails



### **Projeto: Sistema de Classificados com Ruby on Rails**



Este projeto visa criar um sistema de classificados completo e funcional usando a estrutura Ruby on Rails. O sistema permitirá aos usuários criar, gerenciar e pesquisar anúncios classificados.



### **Arquitetura**

#### **Modelo**

- **Classificado:** Representa um único anúncio classificado com atributos como título, descrição, preço, categoria e imagens.
- **Categoria:** Representa uma categoria de classificados, como "Veículos" ou "Imóveis".
- **Usuário:** Representa um usuário do sistema que pode criar e gerenciar anúncios classificados.
- **Imagem:** Representa uma imagem associada a um classificado.



#### **Controle**

- **ClassificadosController:** Gerencia ações relacionadas a classificados, como criação, atualização, exclusão e pesquisa.

- **CategoriasController:** Gerencia ações relacionadas a categorias, como criação e listagem.

- **HomeController:** Gerencia a página inicial do sistema.



#### **Visualização**

- **classificados/index:** Lista todos os classificados.
- **classificados/show:** Exibe um único classificado.
- **classificados/new:** Exibe o formulário de criação de classificado.
- **classificados/edit:** Exibe o formulário de edição de classificado.
- **categorias/index:** Lista todas as categorias.
- **home/index:** Exibe a página inicial.



#### **Fluxo de Trabalho**

1. **Criação de Classificado:** Um usuário acessa a página de criação de classificado e preenche um formulário com os detalhes do classificado, incluindo imagens.

   

2. **Salvamento do Classificado:** O formulário de criação de classificado é enviado e os dados são validados. Se a validação for bem-sucedida, o classificado e as imagens associadas são salvos no banco de dados.

   

3. **Listagem de Classificados:** A página inicial lista todos os classificados salvos.

   

4. **Visualização de Classificado:** Um usuário pode clicar em um classificado na página inicial para visualizar seus detalhes e imagens.

   

5. **Edição de Classificado:** Um usuário pode clicar no botão "Editar" em um classificado para editar seus detalhes e imagens.

   

6. **Atualização do Classificado:** O formulário de edição de classificado é enviado e os dados são validados. Se a validação for bem-sucedida, o classificado e as imagens associadas são atualizados no banco de dados.

   

7. **Exclusão de Classificado:** Um usuário pode clicar no botão "Excluir" em um classificado para excluí-lo do banco de dados.

   

8. **Pesquisa de Classificados:** Os usuários podem pesquisar classificados por título, categoria ou preço usando uma barra de pesquisa na página inicial.



#### **Benefícios**

- **Fácil de Usar:** Ruby on Rails fornece uma sintaxe concisa e intuitiva, tornando o desenvolvimento rápido e fácil.
- **Extensível:** Rails é altamente extensível, permitindo que você adicione facilmente novos recursos e funcionalidades ao seu sistema.
- **Seguro:** Rails inclui recursos de segurança integrados, como proteção contra CSRF e injeção de SQL.
- **Escalável:** Rails é projetado para lidar com grandes quantidades de dados e tráfego, tornando-o adequado para sistemas de grande escala.



### **Conclusão**

Este projeto fornecerá um sistema de classificados completo e funcional que atende aos requisitos de negócios. O uso de Ruby on Rails garante desenvolvimento rápido, extensibilidade, segurança e escalabilidade.



**Código de Exemplo**

#### **Modelo de Classificado**

ruby

```ruby
class Classificado < ApplicationRecord
  belongs_to :categoria
  belongs_to :usuario
  has_many_attached :imagens

  validates :titulo, :descricao, :preco, presence: true
end
```



#### **Modelo de Imagem**

ruby

```ruby
class Imagem < ApplicationRecord
  belongs_to :classificado
end
```



#### **Controle de Classificados**

ruby

```ruby
class ClassificadosController < ApplicationController
  def index
    @classificados = Classificado.all
  end

  def show
    @classificado = Classificado.find(params[:id])
  end

  def new
    @classificado = Classificado.new
  end

  def create
    @classificado = Classificado.new(classificado_params)
    @classificado.usuario = current_user

    if @classificado.save
      redirect_to @classificado, notice: 'Classificado criado com sucesso.'
    else
      render :new
    end
  end

  def edit
    @classificado = Classificado.find(params[:id])
  end

  def update
    @classificado = Classificado.find(params[:id])

    if @classificado.update(classificado_params)
      redirect_to @classificado, notice: 'Classificado atualizado com sucesso.'
    else
      render :edit
    end
  end

  def destroy
    @classificado = Classificado.find(params[:id])
    @classificado.destroy

    redirect_to classificados_url, notice: 'Classificado excluído com sucesso.'
  end

  def search
    @classificados = Classificado.where("titulo LIKE ? OR descricao LIKE ? OR preco = ?", "%#{params[:query]}%", "%#{params[:query]}%", params[:query])
  end

  private

  def classificado_params
    params.require(:classificado).permit(:titulo, :descricao, :preco, :categoria_id, imagens: [])
  end
end
```



#### **Visualização de Classificados (index.html.erb)**

plaintext

```plaintext
<h1>Classificados</h1>

<form>
  <input type="text" name="query" placeholder="Pesquisar...">
  <input type="submit" value="Pesquisar">
</form>

<ul>
  <% @classificados.each do |classificado| %>
    <li><%= link_to classificado.titulo, classificado_path(classificado) %></li>
  <% end %>
</ul>
```



#### **Visualização de Classificado (show.html.erb)**

plaintext

```plaintext
<h1><%= @classificado.titulo %></h1>

<p><%= @classificado.descricao %></p>
<p>Preço: <%= @classificado.preco %></p>
<p>Categoria: <%= @classificado.categoria.nome %></p>

<% if @classificado.imagens.attached? %>
  <ul>
    <% @classificado.imagens.each do |imagem| %>
      <li><%= image_tag imagem.variant(resize: "300x300") %></li>
    <% end %>
  </ul>
<% end %>
```



<h3> Licença</h3>

Este projeto está sob a licença [MIT](./LICENSE).

