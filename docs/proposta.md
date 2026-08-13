ADORA PET — Plataforma esPETacular


1. Visão Geral do Projeto
Tema: Plataforma web para intermediação de adoção responsável de animais de estimação.

Problema que resolve: Facilita a conexão entre pessoas que precisam doar animais e adotantes em potencial, centralizando informações e localidade.

Público-alvo: ONGs, protetores independentes e adotantes.


2. Tecnologias Utilizadas (Tech Stack)

Front-end:
React (com Vite para um ambiente de desenvolvimento rápido e leve).
React Router (para navegação entre páginas).
Axios ou fetch nativo (para consumo da API).
CSS Puro / Tailwind CSS

Back-end:
C# (.NET Core Web API)
Entity Framework Core (EF Core) — ORM para mapeamento e manipulação do banco de dados.

Banco de Dados:
SQLite — Leve, baseado em arquivo único, ideal para projetos acadêmicos por não exigir instalação de servidores complexos.


3. Arquitetura e Modelagem de Dados (Entidades)
Para o escopo do projeto, serão necessárias 3 tabelas principais:

👤 Usuário
Id (Chave Primária - int/Guid)
Cpf (string - único)
Email (string)
SenhaHash (string)
Localidade (string - Ex: Cidade/UF)

🐾 Animal
Id (Chave Primária - int)
Nome (string)
Raca (string)
Idade (int ou string — ex: "2 anos")
Localidade (string)
Vacinado (bool)
Castrado (bool)
Disponivel (bool — para saber se já foi adotado)
UsuarioId (Chave Estrangeira — quem cadastrou o animal)

📝 Solicitação de Adoção (Opcional, mas agrega muito valor)
Id (Chave Primária)
AnimalId (Chave Estrangeira)
AdotanteId (Chave Estrangeira — Usuário que quer adotar)
Mensagem (string — formulário de contato/motivação)
Status (Pendente, Aprovado, Recusado)


4. Funcionalidades (Escopo do MVP)
Autenticação Básica: Cadastro e Login de usuários.
Listagem de Animais: Tela inicial exibindo todos os pets disponíveis para adoção, com filtros simples por localidade ou raça.
Cadastro de Pet: Formulário restrito a usuários logados para cadastrar um novo animal.
Detalhes e Adoção: Visualização completa do pet escolhido e botão para preencher o formulário de contato/interesse.


5. Ideias adicionais

    Classificação de perfil: Pessoa ou ONG 
    Status no perfil: Adotante, Protetor(tem animais para adoção),ONG(tem animais para adoção)
    Adicionais para perfil: Tipo de moradia(casa, apto, casa com quintal)
    Parte dedicada a animais com deficiência ou condições especiais. 
    Animais adotados no mês
    Classificação do perfil: pessoas podem classificar outros usuários caso saibam que se trata de um protetor ou uma pessoa que maltrata animais para evitar possíveis criminosos.

    