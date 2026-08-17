 Proposta do projeto integrador

## Identificação
- Nome provisório: Adora PET

- Integrantes: Silvia Maria Albuquerque Monteiro, Guilherme Inácio dos Santos Moreira, Rafaella Alves Guerra, Sarah Letícia Domingues dos Santos, Daniel Moraes Delgado

- Público usuário: Pessoas que desejam adotar animais

- Problema concreto: Encontrar animais disponíveis para adoção e informações sobre os mesmos é burocrático e dificultoso, muitas vezes não se encontra em plataformas acessíveis, informações são desatualizadas, incompletas ou se perdem pelo caminho. Evitar a compra ilegal de animais, estimulando a adoção responsável.

- Processo atual: Compartilhamento nas redes sociais, como fotos em stories, postagens para compartilhamento, grupos de whatsapp.

- Resultado esperado: Plataforma acessível e centralizada para adoção de animais, com informações atualizadas e completas, sem o risco de perder informações importantes com o compartilhamento.


## Processo principal
Início: O usuário cria um cadastro na plataforma Adora PET.

Decisão: O usuário busca o pet que deseja adotar ou coloca um pet para adoção. 

Mudança de estado: Em caso de adoção, o usuário preenche um formulário de intenção de adoção com seus dados pessoais e de contato. Em caso de disponibilização de pet para adoção, o usuário cria um cadastro para o pet.

Atendimento: O doador visualiza o formulário de intenção de adoção preenchido pelo adotante, obtém os dados de contato (telefone, e-mail, etc.) e entra em contato com o adotante da forma que preferir, fora da plataforma.

Encerramento:O usuário adota o pet e o cadastro do pet é alterado para adotado.


## Conceitos do domínio
| Conceito | Identidade | Estado relevante | Comportamento próprio |
| :-- | :-- | :-- | :-- |
| Usuário  | idUsuario ou cpf | tipoPerfil (ADOTANTE, DOADOR, AMBOS), ativo (Boolean), bloqueado (Boolean)| cadastrarAnimal(), solicitarAdocao(), atualizarPerfil() |
| Animal   | idAnimal | status (PARA_ADOCAO, EM_ADOCAO, ADOTADO, INATIVO) | disponibilizarParaAdocao(), iniciarProcessoAdocao(), concluirAdocao(), retirarDeAdocao()|
| SolicitacaoAdocao | idSolicitacao | status (PENDENTE, APROVADA, RECUSADA, CANCELADA), dataSolicitacao | aprovar(), recusar(), cancelar() |
| Endereço | idEndereco | cep, cidade, estado | validarCep(), formatarEndereco() |
| Notificacao | idNotificacao | status (NAO_LIDA, LIDA), dataEnvio | marcarComoLida(), arquivar() |


## Regras e invariantes
| ID      | Regra | Objetos envolvidos | Sucesso | Falha |
| :-- | :-- | :-- | :-- | :-- |
| REG-001 | Disponibilidade para Intenção de Adoção: Um animal só pode receber novas solicitações de adoção se o seu status atual for PARA_ADOCAO. | Animal, Usuario, SolicitacaoAdocao | Processo de adoção é iniciado e o status do animal muda para EM_ADOCAO. | Lança AnimalIndisponivelException (Ação bloqueada se o pet estiver EM_ADOCAO ou ADOTADO). |
| REG-002 | Autorização de Cadastro: Apenas usuários com conta ativa e perfil configurado (DOADOR ou AMBOS) podem cadastrar um animal.| Usuario, Animal | O animal é cadastrado com sucesso com o status PARA_ADOCAO. | Lança UsuarioInativoOuNaoAutorizadoException. |
| REG-003 | Limite de Solicitações Ativas: Um usuário no papel de ADOTANTE só pode ter no máximo 2 solicitações com status PENDENTE simultaneamente. | Usuario, SolicitacaoAdocao | A solicitação é registrada no sistema. | Lança LimiteSolicitacoesExcedidoException. |
| REG-004 | Restrição de Auto-adoção: O usuário que cadastrou o animal (Doador) não pode solicitar a adoção do próprio animal.| Usuario, Animal | Sistema valida que o ID do solicitante é diferente do ID do responsável pelo cadastro. | Lança AutoAdocaoNaoPermitidaException. |
| REG-005 | Irreversibilidade de Adoção Concluída: Um animal com status ADOTADO não pode retornar para PARA_ADOCAO ou EM_ADOCAO sem um processo explícito de devolução/reativação.| Animal | O status do animal permanece imutável como ADOTADO. | Lança TransicaoEstadoInvalidaException.|
| REG-006 | Invariante de Tutor Único: Um animal só pode ter 1 usuário associado como Doador/Responsável e, após a transição para ADOTADO, exatamente 1 usuário associado como Novo Tutor.| Animal, Usuario | Associação efetuada e histórico registrado no banco. | Lança ConflitoDeResponsavelException.|


## Ciclo de vida
- Objeto central: Animal
- Estados: PARA_ADOCAO, EM_ADOCAO, ADOTADO, INATIVO
- Transições permitidas: disponibilizarParaAdocao(), iniciarProcessoAdocao(), concluirAdocao(), retirarDeAdocao()
- Transições proibidas: ADOTADO ➔ PARA_ADOCAO (REG-005); PARA_ADOCAO ➔ ADOTADO (deve passar pelo estado intermediário de análise em EM_ADOCAO); ADOTADO ➔ INATIVO (REG-005).


## Fluxos da versão final
| ID | Ação do usuário | Regra principal | Alteração persistida | Resultado |
| :-- | :-- | :-- | :-- | :-- |
| FLX-001 | Cadastro de usuário: Preenche o formulário informando os dados pessoais, endereço e escolhe o tipo de perfil (ADOTANTE, DOADOR, AMBOS). | Validação de CPF único e formato do e-mail | Insere registro na tabela usuário (ativo = True) e vincula um Endereço | Usuário criado e logado no sistema |
| FLX-002 | Cadastro do PET para adoção: Doador preenche a ficha do animal (nome, espécie, idade, saúde, vacinas, castrado, foto, descrição, raça, localização) | REG-002 (Autorização de Doador ativo) e REG-006 (Vinculação de Tutor Único Inicial) | Insere registro na tabela animal com status PARA_ADOCAO e vincula ao doador | Pet cadastrado e disponível para adoção |
| FLX-003 | Busca e filtragem dos PETS: Usuário pode navegar utilizando filtros (espécie, porte, idade, saúde, vacinas, castrado, raça, localização) |Exibição restrita a animais atualmente disponíveis (status == PARA_ADOCAO) | Nenhuma (Operação de leitura/consulta otimizada) | Pets filtrados exibidos na interface de busca |
| FLX-004 | Solicitação de Adoção: Adotante clica em "quero adotar", preenche o formulário de intenção de adoção (nome, telefone, e-mail, motivo, experiência com animais) e envia a proposta | REG-001 (Pet PARA_ADOCAO), REG-003 (Máx 2 ativas) e REG-004 (Sem Auto-adoção) | Registra em SolicitacaoAdocao (PENDENTE); Atualiza Animal.status para EM_ADOCAO | Solicitação registrada. Sistema gera uma Notificacao para o doador e o pet entra em análise reservada |
| FLX-005 | Visualização da Intenção de Adoção: Doador acessa a lista de solicitações recebidas para seus pets e visualiza os dados do formulário preenchido pelo adotante (nome, telefone, e-mail, motivo) | REG-002 (Doador ativo e responsável pelo animal) | Nenhuma (Operação de leitura) | Doador obtém os dados de contato do adotante e pode entrar em contato fora da plataforma (telefone, e-mail, etc.) |
| FLX-006 | Conclusão da Adoção: Após contato e acordo direto com o adotante, o doador atualiza o status do animal para ADOTADO (EM_ADOCAO -> ADOTADO) | REG-005 (Irreversibilidade de Adoção Concluída) e REG-006 (Invariante de Tutor Único) | Atualiza Animal.status para ADOTADO, vincula ao novo tutor e atualiza SolicitacaoAdocao.status para APROVADA | Pet adotado e indisponível para novas solicitações |
| FLX-007 | Recusa/Cancelamento: Doador recusa a solicitação ou adotante desiste da adoção | REG-005 (Guarda: bloqueia ação se Animal já estiver ADOTADO) | Atualiza SolicitacaoAdocao.status para RECUSADA ou CANCELADA; Reverte Animal.status de EM_ADOCAO para PARA_ADOCAO | Solicitação encerrada. Animal volta a ficar disponível para novas solicitações |
| FLX-008 | Retirada do PET da plataforma: Doador desiste de disponibilizar o animal (desistência, cadastro errado ou outro motivo) | REG-005 (Guarda: bloqueia ação se Animal já estiver ADOTADO) | Atualiza Animal.status para INATIVO; Cancela solicitações PENDENTES vinculadas ao animal | Pet removido da listagem pública e indisponível para novas solicitações |


## Variação polimórfica
- O que varia: A forma como o doador é notificado sobre uma nova solicitação de adoção (FLX-004).
- Contrato possível: `Notificador.notificar(doador, solicitacao)` — interface que recebe o doador e a solicitação e executa a notificação.
- Implementação 1: `NotificadorEmail` — envia um e-mail ao doador informando que há uma nova solicitação de adoção para seu pet.
- Implementação 2: `NotificadorPlataforma` — exibe uma notificação interna na plataforma (badge/alerta no painel do doador).
- Por que a variação é legítima: Diferentes doadores podem preferir canais de notificação distintos; o comportamento de "notificar" é o mesmo, mas o meio de entrega varia. A interface permite adicionar novos canais (ex: SMS) sem alterar a lógica de negócio.


## Escopo da AV1
- Fatia vertical escolhida: Cadastro de Usuário (FLX-001) + Cadastro de Pet (FLX-002) + Solicitação de Adoção (FLX-004). Essa fatia cobre o caminho completo desde a criação de conta até a primeira mudança de estado do animal.
- Três regras essenciais: REG-001 (Animal deve estar PARA_ADOCAO), REG-002 (Doador ativo para cadastrar), REG-004 (Sem auto-adoção).
- Mudança de estado: PARA_ADOCAO ➔ EM_ADOCAO (disparada pelo FLX-004 quando o adotante envia a solicitação).
- Dados persistidos: INSERT em Usuário (ativo=true, bloqueado=false), INSERT em Animal (status=PARA_ADOCAO, vínculo com doador), INSERT em SolicitacaoAdocao (status=PENDENTE), UPDATE em Animal.status para EM_ADOCAO.


## Escopo da AV2
- Evoluções previstas: Busca com filtros (FLX-003), Visualização do formulário pelo doador (FLX-005), Conclusão da adoção (FLX-006), Recusa/Cancelamento (FLX-007) e Retirada do pet (FLX-008).
- Três fluxos completos: FLX-003 (Busca e filtragem), FLX-006 (Conclusão da adoção — EM_ADOCAO ➔ ADOTADO), FLX-007 (Recusa/Cancelamento — EM_ADOCAO ➔ PARA_ADOCAO).
- Falhas tratadas: AnimalIndisponivelException (REG-001), LimiteSolicitacoesExcedidoException (REG-003), AutoAdocaoNaoPermitidaException (REG-004), TransicaoEstadoInvalidaException (REG-005), ConflitoDeResponsavelException (REG-006).
- Testes esperados: Testes unitários para cada regra (REG-001 a REG-006), teste de integração do ciclo completo (PARA_ADOCAO ➔ EM_ADOCAO ➔ ADOTADO), teste de recusa com retorno a PARA_ADOCAO, teste de retirada com cancelamento de solicitações pendentes.


## Fora do escopo
Doações financeiras, logística/transporte físico do pet e integração com bancos de dados governamentais


## Riscos e mitigação
| Risco | Impacto | Mitigação |
| :-- | :-- | :-- |
| Dados falsos no formulário de intenção (telefone/e-mail inválidos) | Doador não consegue contatar o adotante | Validação de formato no front-end e obrigatoriedade de campos essenciais |
| Pet preso em EM_ADOCAO por inação do doador | Adotante fica sem resposta e pet sai da busca | Prazo limite (ex: 15 dias); após expiração, solicitação é cancelada automaticamente e pet volta a PARA_ADOCAO |
| Sobrecarga de solicitações para um pet popular | Frustração de adotantes que não serão atendidos | REG-003 (limite de 2 solicitações ativas por adotante) |
| Membro do grupo indisponível em fase crítica | Atraso na entrega da AV1 ou AV2 | Rodízio de tarefas e pareamento; cada funcionalidade tem ao menos 2 pessoas que conhecem o código |
| Perda de dados por falha técnica | Cadastros e solicitações perdidos | Backups periódicos do banco de dados e versionamento do código (Git) |


## Participação

- **Rodízio**: Cada integrante será responsável principal por ao menos um fluxo (FLX) na AV1 e na AV2, alternando entre back-end e front-end a cada ciclo.
- **Revisões cruzadas**: Todo pull request deve ser revisado por pelo menos 1 integrante que não participou da implementação, garantindo que todos leiam código de áreas diferentes.
- **Conhecimento compartilhado**: Reuniões semanais de 30 min para demonstração do que cada um desenvolveu, garantindo que todos conheçam o produto inteiro.