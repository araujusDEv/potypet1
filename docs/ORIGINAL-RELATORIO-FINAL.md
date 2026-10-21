# PotyPet 4.7.0 — revisão final local

Revisão iniciada em 13/10/2026; entrega em **14/10/2026** (America/Sao_Paulo). Base: PotyPet-documentos-4.7.0-rc.6-2026-10-13.zip, SHA-256 **53635e852f48b623603f049def803f8e2d63c8a162ccccf6b5bcb3b121fda089**, correspondente à entrega anterior. Todos os 287 hashes de entrada conferidos. Não houve retorno ao ZIP original, prévia visual, publicação, contratação ou acesso a banco operacional.

## Resultado e alteração desta rodada

**24 suítes existentes passaram, saída 0; 288 cenários de navegador e um grupo API adicional passaram; 456 auditorias axe passaram nos critérios executados.** Não foi encontrada regressão funcional dos fluxos avaliados que exigisse alteração do código de produção. A 4.7.0 atualiza versão/documentação e acrescenta evidências da revisão; JS/CJS, testes existentes, páginas públicas, estilos, imagens, fontes, esquema, dependências, cookies, rotas, chaves e arquivos do Render permanecem byte a byte em relação à rc.6. São 10 arquivos existentes modificados e dois documentos novos; lista em ARQUIVOS-MODIFICADOS.md. Ferramentas/evidências QA ficam separadas do backend.

Termos públicos ainda são revisão 2026-10-13-potypet-rc6-revisao, porque não foram substituídos nesta conferência. Versão do software e versão dos documentos são distintas. Não houve aceite retroativo, login automático ou redução da confirmação de e-mail. Recibos, cadastros pendentes, contratos, conversas, evidências e auditorias antigas permanecem. Appet subsiste em origem histórica e identificadores técnicos compatíveis.

## Método e suítes

Runtime executado: **Node 24.21.0, Windows x64**. npm ci foi reexecutado no runtime correto antes dos testes. A primeira tentativa de instalação usou Node 22 por resolução incorreta do caminho; nenhum resultado de teste no Node 22 foi usado como aprovação. Node 24 consta como LTS e a publicação 24.21.0 foi conferida em fontes oficiais em 13/10/2026: [releases Node](https://nodejs.org/en/about/previous-releases), [24.21.0 LTS](https://nodejs.org/en/blog/release/v24.21.0). Requisitos de WAL foram conferidos na [documentação SQLite](https://www.sqlite.org/wal.html). Isso não comprova capacidade de qualquer hospedagem.

npm test: pix, workflow, security, security-regressions, migration, brand, admin-recovery, qa-regressions, email-auth, backups, etapa1, etapa2, etapa3, etapa4, etapa5, etapa6, brand-history, concurrency, admin-actions, account-restrictions, account-history-ui, photo-approval, photo-concurrency, review-operations. Brand/brand-history foram repetidos após atualizar a documentação. Sintaxe de 93 JS/CJS passou; scanner de padrões reconhecidos de segredo/dados operacionais sem achados. O scanner não é promessa de detecção de todos os segredos.

**API/SQLite:** rotas HTTP reais em processos locais com bancos fictícios novos para aprovação, login, privacidade, moderação, suspensão, adoção, campanhas e e-mail; ensaios de migração/backup/recuperação. Barreiras IPC controlam processamento de foto/PDF para conferir novo cadastro, ocultação, outra edição, revogação de sessão e revalidação. Alguns casos legados injetam estados por SQL no banco descartável do próprio teste; não representam uma ação permitida na interface ou acesso ao banco de uma instalação em uso. O teste de perfil ONG com microtask e os estados forçados são explicitamente sintéticos.

**Módulos/VM:** Pix/CRC e validações; admin-actions tem 47 grupos com respostas 200/403/409/503 e Promises controladas; account-history-ui tem nove grupos de hydrate real em VM. Não chamar esses grupos de ensaio HTTP de hospedagem. Integridade tem 19 grupos e restrição 39, conforme JSON/logs.

**Navegador:** Playwright-core 1.57.0 com Chrome instalado, axe-core 4.10.3, viewport 360/1366, temas claro/escuro. Ações feitas no DOM real e servidor local. 503/atrasos específicos são controlados pelo transporte; 200, 403 e 409 administrativos incluem execução contra o servidor local. Os roteiros herdados receberam fixtures legítimas; preparação de datas/papéis de navegador ocorre com o servidor fictício parado. A matriz adicional prepara estado pela API, sem SQL direto, e separa clientes por proxy localhost restrito; limites do servidor não foram desativados.

**Inspeção manual:** inspeção visual pelo assistente de oito capturas listadas em qa/manual-inspection.json, incluindo início, perfil em análise, aprovação de ONG e regra mínima. Não foi sessão de uso manual em aparelho físico nem conferência humana de documentos/animais. Em 360 px a tabela administrativa tem rolagem horizontal interna. Modais têm conteúdo rolável. Marca, estados e temas foram examinados nas capturas indicadas.

## Matriz executada

| Viewport / tema | Cenários de navegador | Auditorias axe | Resultado |
|---|---:|---:|---|
| 360 light | 72 | 114 | Passou |
| 360 dark | 72 | 114 | Passou |
| 1366 light | 72 | 114 | Passou |
| 1366 dark | 72 | 114 | Passou |

| Conjunto | Grupos/cenários | Auditorias | Resultado |
|---|---:|---:|---|
| documents | 28 | 36 | Passou |
| photos | 28 | 44 | Passou |
| history | 64 | 88 | Passou |
| admin | 88 | 64 | Passou |
| adoption | 20 | 100 | Passou |
| experience | 24 | 32 | Passou |
| campaigns | 20 | 28 | Passou |
| final | 17 | 64 | Passou |

O conjunto final tem 16 cenários de navegador e um grupo API. Cada cenário contém várias verificações; números não significam quantidade de asserts. JSON completos/capturas em qa/, sem e-mails de teste ou banco.

Zero violações axe nas 456 auditorias, nos tags WCAG2 A/AA e WCAG2.1 A/AA executados. Monitores de pageerror registraram zero em **448** auditorias; oito capturas administrativas de fotos não têm esse monitor. Rejeições sem tratamento foram monitoradas e ficaram zeradas em **288** auditorias, incluindo os conjuntos administrativos/históricos; roteiros antigos de adoção/experiência/campanha não têm monitor global de unhandledrejection. Campos ausentes são limitação, não aprovação. Não é certificação completa WCAG nem teste em Safari, Android/iOS físico ou leitor de tela.

## Falhas iniciais e ajustes de ensaio

Não houve falha funcional na suíte existente. Os novos/antigos roteiros QA tiveram erros de preparação, conservados em logs: opção inválida other_pets=nenhum (corrigida para nao); tentativa de adicionar cookie vazio a visitante; múltiplos logins do mesmo IP atingindo o limite de dez; animal antigo de roteiro sem descrição; foto alterada que corretamente voltou à análise; tentativa de ler payload já removido da fila após envio local. Esses casos foram corrigidos no ensaio, mantendo validações, aprovação, limites e redução de dados da fila. O teste de fila recuperada passou a criar mensagem pendente pela API antes do snapshot. Conjuntos afetados foram repetidos até passar. O agregador também passou a distinguir monitores ausentes, sem tratá-los como zero erros.

## Atualização e recuperação local

Ensaio rc.6 → 4.7.0 em base fictícia: snapshot de schema/contas/senhas/aceites, foto enviada, vínculos, solicitação concluída, contrato, conversa, suspensão, sessões, fila e cadastro pendente preservados. Confirmação de token realmente emitido na rc.6 mantém versão/data de aceite e não cria sessão. Dois reinícios mantêm dados e restrição; não houve aviso antigo reenfileirado por atualizar.

Ferramentas reais de snapshot/criptografia/decrypt/restore-check recuperaram a base em diretório novo; bytes/estado conferidos antes de iniciar. Destino não vazio rejeitado. Após iniciar, login/histórico/autorização continuam e mensagem pendente é lida com MAIL_QUEUE_KEY original e aceita pelo transporte file. BACKUP_ENCRYPTION_KEY correspondente foi mantida no ensaio. Isso comprova recuperação **local fictícia**, não download de serviço externo ou recuperação após perda do provedor. Consulte qa/upgrade-results.json.

## Classificação por item

| Item | Classificação | Evidência / limite |
|---|---|---|
| Renomeação pública e logotipo | implementado e testado | 33 páginas, novos termos/e-mails, compartilhamento e preservação da origem histórica; brand + brand-history e capturas. |
| Integridade em operações concorrentes | implementado e testado | 19 regressões de integridade e revisão de fotos; barreiras controladas, HTTP/SQLite e casos de perda de sessão/papel. |
| Feedback administrativo | implementado e testado | 47 grupos em VM e 88 no navegador; 200, 403, 409, 503, refresh atrasado, botão ocupado e clique duplicado. |
| Suspensão, reativação e autorização | implementado e testado | 39 grupos do servidor; sessão revogada, escrita bloqueada, histórico, administradores protegidos e privilégio não concedido. |
| Navegação histórica | implementado e testado | 9 grupos de hydrate em VM e conjunto navegador para PF suspensa, ONG suspensa/reprovada; terceiros sem vínculo rejeitados. |
| Fotos e mínimo para novas aprovações | implementado e testado | JPG/PNG/WebP, EXIF/orientação, ausência de GPS na saída, >2 MB, corrupção, limites, edição sem fotos e exceção registrada. |
| Autenticidade da foto, caso e evidência | pendente | Requer conferência humana com responsáveis e fontes; validade técnica não prova correspondência nem detecta IA com certeza. |
| Aprovação/disponibilidade e vitrines | implementado e testado | Pendente/reprovado fora de busca/destaque/recomendação, dono vê estado e não pede adoção própria; processo/adotado sem novos pedidos. |
| Indicadores públicos | implementado e testado | Stats, catálogo, home e transparência coerentes; só aprovados/disponíveis e organizações públicas elegíveis. Conclusão separada de aprovação, termo e adoção externa. |
| Retorno após login/cadastro | implementado e testado | Cadastro animal/desaparecido, área pessoal, mesmo ID do perfil e alternância login/cadastro; externo/inválido rejeitado; sem autenticação automática. |
| Privacidade dos pedidos, mensagens e acompanhamentos | implementado e testado | Somente partes/admin autorizados; contato por consentimento, revogação e detalhes fora do público; recusa/cancelamento/histórico preservados. |
| Entrega, recebimento, conclusão e termos | implementado e testado | Duas contas distintas; ordem/transições/duplicações conferidas; concordância separada e sem apresentar assinatura certificada ou prova independente. |
| Acompanhamentos 7/30/90 e ajuda | implementado e testado | Prazos em dados fictícios, relatos privados, pedido/resposta de ajuda e bloqueio de repetição; não certifica socorro ou atendimento humano. |
| Campanhas e Pix no servidor | implementado e testado | Evidências privadas, revisão por assunto, fonte, resumo público, nova chave/valores pendentes, pausa/encerramento e independência da adoção. |
| Pagamento e confirmação bancária real | pendente | Payload/Pix e valores manuais são testados localmente; nenhum pagamento ou confirmação bancária foi executado. |
| Notificações/fila de e-mail | implementado com verificação limitada | Registro, tentativa e aceitação file/controlada conferidos; novo anúncio/decisão com links autorizados e conteúdo genérico. |
| SMTP e recebimento real | pendente | Definir remetente/transporte/credenciais no ambiente e comprovar confirmação/recuperação/eventos em caixas autorizadas. |
| Documentação independente de hospedagem | implementado e testado | Guias/requisitos/configuração explicitam Node/SQLite, instância única, disco, HTTPS, PUBLIC_BASE_URL, SMTP/proxy/backups; Render opcional inalterado. |
| Runtime, CI e sistema do destino | implementado com verificação limitada | Node 24.21.0 Windows executado; versão/engines/CI alinhados. CI remoto, Linux e dependências no provedor ainda não executados. |
| Atualização, migração idempotente e reinício locais | implementado e testado | rc.6 → 4.7.0 e bases antigas fictícias; schema e registros preservados; dois reinícios sem novo aceite. |
| Backup e recuperação criptografada locais | implementado e testado | Snapshot consistente, integridade, chaves, fila, destino novo e proteção de destino existente; ferramentas reais com SQLite fictício. |
| Exportação S3 e backup externo operacional | implementado com verificação limitada | Cliente/transporte S3 fictício na suíte; nenhum bucket real, download externo ou recuperação após perda do provedor. |
| Hospedagem, HTTPS/proxy e persistência operacional | pendente | Produto não escolhido; comprovar disco/redeploy, IP confiável, cookies, SMTP e recuperação no destino antes de usuários reais. |
| 360/1366 px, claro/escuro | implementado e testado | 72 cenários e 114 auditorias em cada combinação no Chrome; limites de inspeção visual e monitores discriminados. |
| Dispositivos físicos e acessibilidade completa | implementado com verificação limitada | Axe/teclado/capturas conferidos; sem aparelho físico, Safari ou leitor de tela; tabela admin usa rolagem horizontal. |
| Responsáveis, conteúdo final e atendimento | pendente | Equipe principal/privacidade/retencão/substituto a definir; Gmail e disponibilidade informados não comprovam serviço operacional. |

## Decisão e pendências de abertura

**Pronto para piloto técnico local ou homologação com dados fictícios; ainda bloqueado para operação pública com usuários reais.** Corrigir as seguintes pendências antes dessa abertura:

1. Definir responsável principal e decisões sobre dados, substituto, critérios de conservação, moderação/denúncia/contestação/ajuda e atendimento; finalizar campos [A DEFINIR] dos documentos e identificar sua versão/comunicação sem reescrever aceites. A operação pelos alunos não atribui responsabilidade à escola.
2. Conferir acesso e atendimento do Potypett@gmail.com, horários/triagem e canal alternativo; sexta a domingo é disponibilidade informada, não prazo garantido. Definir remetente/SMTP e comprovar recebimento real de confirmação/recuperação/eventos e links autorizados.
3. Escolher produto compatível com Node/SQLite, instância única e volume persistente. Comprovar permissões, restart/redeploy, HTTPS/cookies, PUBLIC_BASE_URL, proxy/IP e limites no destino. R$ 150 é total para cinco a sete meses; não há plano selecionado ou custo certificado.
4. Configurar backup privado separado, espaço, retenção e acessos; baixar e restaurar em destino novo, guardando chaves correspondentes fora do serviço. Exportação S3 do teste é simulada.
5. Conferir fotos reais autorizadas, direitos das imagens existentes, autenticidade de anúncios/campanhas e fontes humanas independentes. Pix/valores manuais não comprovam pagamento bancário; qualquer ensaio financeiro real permanece pendente.

Não houve SMTP real, inbox real, pagamento, contato externo com clínica/fornecedor, hospedagem/persistência ou recuperação externa real nesta revisão. Testes de carga extensivos/penetração completa, CI remoto, Linux/Safari/dispositivo físico e leitor de tela não foram executados. Guias e exemplos preparam a revisão; não equivalem a contratação, deploy ou selo de segurança total.

## Atualizar sem perder dados

Leia appet/ATUALIZACAO-4.7.0.md: backup consistente e cópia privada externa; parar serviço; substituir somente código, preservar DATA_DIR e MAIL_QUEUE_KEY/BACKUP_ENCRYPTION_KEY/configurações, não importar seed nem recriar banco. Instalar com npm ci no runtime/plataforma corretos; conferir contas, fotos, vínculos e históricos antes/depois/reinício. Recuperação em destino novo com chaves correspondentes. Leia appet/GUIA-HOSPEDAGEM.md e appet/PENDENCIAS-OPERACIONAIS.md para conteúdo/produção.

O manifesto cobre todos os arquivos, exceto ele próprio para evitar autorreferência. Hash do ZIP é externo. Não incluem node_modules, bancos operacionais ou dos ensaios, segredos/chaves, caches, cookies, backups ou e-mails gerados. Seed e fixtures necessários são identificados em EXEMPLOS-E-DADOS.md. Relatórios anteriores e manifesto rc.6 são origem histórica, não resultados desta execução.
