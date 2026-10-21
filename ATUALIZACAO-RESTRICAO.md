# Migração — PotyPet 4.7.0-rc.4

Entrega intermediária sobre o ZIP PotyPet rc.3. SHA-256 recebido: `9a1c29afaeb2b1c8297e421477e7cc3abc6623d8fadc788dce5d3790673e396c`, conferido com a entrega anterior. Registro em `../RECEBIMENTO.json`; resultados em `../RELATORIO-RESTRICAO.md`. A final 4.7.0 depende da revisão final.

## Substituir o código, preservar o banco

1. Pare a aplicação. Faça backup consistente de todo `DATA_DIR`, banco `adotapet.db`, fotos, anexos, históricos e arquivos WAL/SHM que existirem. Guarde também as configurações e as chaves da fila de e-mail e dos backups. Confira a restauração em cópia separada.
2. Extraia este ZIP em uma pasta nova. Substitua somente o código de `appet/`, incluindo as bibliotecas novas, servidor, JS, HTML com versões de cache, package/lock e testes. Preserve o diretório de dados ativo inteiro. Se usa `appet/data` local, não sobrescreva essa pasta nem reimporte o seed.
3. Mantenha `DATA_DIR`, endereços, variáveis, credenciais, cookies e chaves existentes. Use Node 24.21.0 e `npm ci`. Em homologação isolada, `npm test` executa 21 suítes; `npm run test:restriction` executa os dois conjuntos novos. Não use banco real nos testes.
4. Ao iniciar, o servidor acrescenta somente quatro colunas à tabela `users`: `account_suspended_at`, `account_suspended_by`, `account_suspension_reason` e `account_moderation_history`. Valores iniciais: NULL, NULL, texto vazio e `[]`. Cada coluna é conferida antes de acrescentar; reinícios repetidos não apagam decisões nem repetem a migração. Nenhuma conta, foto, coleção, contrato ou mensagem é recriada.
5. Confira acesso com senha e e-mail confirmado, decisões administrativas, consulta histórica e bloqueios de escrita. Suspensões/reprovações anteriores de ONG continuam válidas. As novas colunas não alteram `approval_status`, papel ou confirmação de e-mail. Cache novo do JS/HTML deve ser mantido.

O código rc.3 não interpreta a suspensão comum acrescentada nesta etapa. **Não volte ao código anterior com contas já suspensas pelo mecanismo novo**, pois ele não aplicaria esse bloqueio. Qualquer retorno precisa de código compatível com a restrição e revisão da restauração; não descarte dados criados após a atualização.

## Operação administrativa

No painel, escolha **Suspender conta**, informe a justificativa e confirme. Sessões do titular são revogadas. O titular pode entrar novamente com senha e e-mail confirmado para consultar somente seus registros autorizados, em modo restrito. A decisão registra administrador, data e motivo; esses detalhes ficam para o titular e a administração.

**Reativar conta** exige justificativa, registra uma nova decisão e revoga também sessões de consulta existentes. O titular precisa fazer novo login. A operação não promove papel, não confirma e-mail, não cria aprovação para pessoa física e não aprova ONG pendente/reprovada. A análise cadastral da ONG continua uma ação separada. Contas administrativas, incluindo a própria e o último administrador, são protegidas destas operações.

As rotas autenticadas de escrita são bloqueadas, inclusive mensagens, termos, pedidos, anúncios, acompanhamentos e arquivos. Históricos não são excluídos. Campanhas e vitrines públicas continuam dependentes de seus critérios; suspensão do organizador impede elegibilidade pública, sem apagar sua versão aprovada ou depender da disponibilidade de um animal. Desaparecidos seguem a política pública/moderação existente.

## Limite da suspensão

**Suspender a conta não identifica a pessoa depois que ela sai dela.** Ela pode acessar como visitante as rotas anônimas existentes, sujeitas às mesmas validações, limites por IP e demais controles. Cadastro, confirmação/recuperação de e-mail, contato, denúncias, avistamentos, consulta pública e Pix público elegível conservam sua política. Não existe promessa de bloquear absolutamente uma pessoa, outros dispositivos ou uma nova conta.

Sem publicação, domínio/hospedagem alterados, SMTP real ou pagamento. Testes usam dados fictícios, e-mails locais e Chrome automatizado. Persistência no provedor, aparelho físico/leitor de tela, carga de produção e a revisão final ainda dependem de verificação própria.
