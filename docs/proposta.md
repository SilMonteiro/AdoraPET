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

Mudança de estado: Em caso de adoção, o usuário preenche um formulário de interesse e pode enviar mensagens ao tutor responsável pelo pet. Em caso de disponibilização de pet para adoção, o usuário cria um cadastro para o pet.

Atendimento: O tutor responsável pelo pet recebe as mensagens e pode responder. 

Encerramento:O usuário adota o pet e o cadastro do pet é alterado para adotado.

## Conceitos do domínio
| Conceito | Identidade | Estado relevante | Comportamento próprio |
| :-- | :-- | :-- | :-- |
| Usuário  | idUsuario ou cpf | tipoPerfil (ADOTANTE, DOADOR, AMBOS), ativo (Boolean), bloqueado (Boolean)| cadastrarAnimal(), solicitarAdocao(), atualizarPerfil() |
| Animal   | idAnimal | status (PARA_ADOCAO, EM_ADOCAO, ADOTADO) | disponibilizarParaAdocao(), iniciarProcessoAdocao(), concluirAdocao(), retirarDeAdocao()|

## Regras e invariantes
| ID      | Regra | Objetos envolvidos | Sucesso | Falha |
| :-- | :-- | :-- | :-- | :-- |
| REG-001 | Disponibilidade para Intenção de Adoção: Um animal só pode receber novas solicitações de adoção se o seu status atual for PARA_ADOCAO. | Animal, Usuario | Processo de adoção é iniciado e o status do animal muda para EM_ADOCAO. | Lança AnimalIndisponivelException (Ação bloqueada se o pet estiver EM_ADOCAO ou ADOTADO). |

| REG-002 | Autorização de Cadastro: Apenas usuários com conta ativa e perfil configurado (DOADOR ou AMBOS) podem cadastrar um animal.| Usuario, Animal | O animal é cadastrado com sucesso com o status PARA_ADOCAO. | Lança UsuarioInativoOuNaoAutorizadoException. |

| REG-003 | Limite de Solicitações Ativas: Um usuário no papel de ADOTANTE só pode ter no máximo 2 solicitações com status EM_ADOCAO simultaneamente. | Usuario (Adotante), Animal | A solicitação é registrada no sistema. | Lança LimiteSolicitacoesExcedidoException. |

| REG-004 | Restrição de Auto-adoção: O usuário que cadastrou o animal (Doador) não pode solicitar a adoção do próprio animal.| Usuario, Animal | Sistema valida que o ID do solicitante é diferente do ID do responsável pelo cadastro. | Lança AutoAdocaoNaoPermitidaException. |

| REG-005 | Irreversibilidade de Adoção Concluída: Um animal com status ADOTADO não pode retornar para PARA_ADOCAO ou EM_ADOCAO sem um processo explícito de devolução/reativação.| Animal | O status do animal permanece imutável como ADOTADO. | Lança TransicaoEstadoInvalidaException.|

| REG-006 | Invariante de Tutor Único: Um animal só pode ter 1 usuário associado como Doador/Responsável e, após a transição para ADOTADO, exatamente 1 usuário associado como Novo Tutor.| Animal, Usuario | Associação efetuada e histórico registrado no banco. | Lança ConflitoDeResponsavelException.|


## Ciclo de vida
- Objeto central:
- Estados:
- Transições permitidas:
- Transições proibidas:

## Fluxos da versão final
| ID | Ação do usuário | Regra principal | Alteração persistida | Resultado |
| :-- | :-- | :-- | :-- | :-- |
| FLX-001 | | | | |

## Variação polimórfica
- O que varia:
- Contrato possível:
- Implementação 1:
- Implementação 2:
- Por que a variação é legítima:

## Escopo da AV1
- Fatia vertical escolhida:
- Três regras essenciais:
- Mudança de estado:
- Dados persistidos:

## Escopo da AV2
- Evoluções previstas:
- Três fluxos completos:
- Falhas tratadas:
- Testes esperados:

## Fora do escopo
-

## Riscos e mitigação
| Risco | Impacto | Mitigação |
| :-- | :-- | :-- |
| | | |

## Participação
Explique o rodízio, as revisões cruzadas e como todos conhecerão o produto inteiro.