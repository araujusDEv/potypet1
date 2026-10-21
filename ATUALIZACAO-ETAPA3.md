> Registro histórico. Para esta entrega, use Node 24.21.0 LTS e siga [ATUALIZACAO-ETAPA6.md](ATUALIZACAO-ETAPA6.md) e [PUBLICAR-HOJE.md](PUBLICAR-HOJE.md). Resultados abaixo pertencem à etapa indicada, não à conferência atual.

# Atualização para Appet 4.3.0 — 12/10/2026

Documento histórico preservado. Para esta entrega 4.4.0, siga **ATUALIZACAO-ETAPA4.md** e **../RELATORIO-ETAPA4.md**; as instruções e os resultados atuais prevalecem sobre os caminhos de testes desta referência.

Esta entrega continua a versão 4.2.0 da etapa 2. Preserve o banco existente. Não importe o seed de demonstração sobre dados reais e não execute os testes apontando para produção.

1. Pare o serviço antes da troca. Faça uma cópia consistente de **todo o DATA_DIR** com o serviço parado: banco SQLite e eventuais arquivos WAL/SHM, uploads privados, fila de e-mail, arquivos de campanhas e demais arquivos vinculados. Guarde também as configurações do ambiente e os backups existentes. Confirme que a cópia pode ser restaurada.
2. Extraia o ZIP em uma pasta separada. A pasta `appet` contém o código completo. Substitua o código pelos arquivos desta pasta, incluindo os novos `lib/adoption-workflow.cjs`, `js/adoption-ui.js` e os documentos atualizados. Preserve seu DATA_DIR e suas pastas de dados; a entrega não contém banco operacional.
3. Mantenha as variáveis de ambiente atuais, especialmente DATA_DIR, MAIL_QUEUE_KEY, SMTP, PUBLIC_BASE_URL, COOKIE_SECURE e credenciais administrativas. **Não troque a MAIL_QUEUE_KEY com e-mails pendentes**, pois ela é necessária para ler a fila existente. Não copie os endereços fictícios dos testes para a hospedagem.
4. Na nova pasta de código, use Node.js compatível (testado com 22.23.1) e execute `npm ci`. A entrega não inclui node_modules. Os novos campos são adicionados aos registros quando as ações acontecem; não é necessário recriar o banco ou editar as coleções manualmente.
5. Inicie o serviço com suas configurações reais. Em produção mantenha HTTPS, COOKIE_SECURE=1, EMAIL_TRANSPORT=smtp, DATA_DIR persistente fora do código e demonstrações desabilitadas. Se sua instalação é local e usa o caminho padrão `appet/data`, restaure/preserve o conteúdo operacional dessa pasta antes de iniciar; só o seed está no pacote.
6. Confira o login, os animais e pedidos antigos, contratos, acompanhamentos, campanhas e arquivos. Teste com duas contas separadas de homologação. O novo fluxo é: interesse → aprovação → entrega informada pelo responsável → recebimento confirmado pelo interessado → conclusão pelo responsável → leitura/concordância do termo por cada parte → acompanhamentos de 7, 30 e 90 dias.
7. Configure o remetente oficial quando sua conta de e-mail estiver criada. Verifique o recebimento real da confirmação, recuperação e dos avisos dos eventos em caixas externas. Os testes entregues usaram **transporte local em arquivo**, não comprovaram entrega SMTP real. Observe a fila e falhas no painel administrativo. O formulário de contato com protocolo continua sendo o canal existente; nenhum e-mail pessoal foi publicado como atendimento oficial.

Pedidos concluídos e termos antigos são preservados. Nos termos anteriores, a confirmação do responsável pode ter sido gerada automaticamente junto com a conclusão; a interface explica essa diferença. A atualização não inventa entrega, recebimento ou novos aceites para registros antigos. O indicador continua contando pedidos com status `concluida`, incluindo o histórico existente; aprovações, confirmações do termo e adoções externas não acrescentam adoções.

Formulários e mensagens ficam nas áreas autenticadas. E-mail e telefone só são mostrados à outra parte se houver autorização por pedido; pedidos antigos sem essa autorização não recebem consentimento retroativo. A revogação não apaga cópias já obtidas ou mensagens enviadas. ONGs suspensas continuam com consulta restrita; a administração trata o histórico.

Após uma entrega registrada, rejeição, cancelamento, retirada e arquivamento ficam bloqueados enquanto a conclusão estiver pendente. Problemas reais, devolução ou divergência devem ser tratados pela conversa privada e pelo atendimento administrativo; esta etapa não implementa um fluxo completo de devolução ou disputa. Depois de concluir, o anúncio pode ser arquivado e as partes mantêm seu histórico.

Para verificar uma cópia isolada: `npm test` executa 13 suítes; `npm run test:etapa3` executa os testes específicos desta etapa com contas e banco fictícios em uma pasta temporária. O executor de navegador está em `../qa/run-etapa3.cjs` e requer Playwright Core, axe-core e Chrome. Os testes não devem ser usados para inserir dados no banco real.

Em caso de falha na atualização, pare o serviço, restaure o código anterior junto com a cópia consistente dos dados/configurações, e confira os registros antes de reabrir. Não continue alternando versões sobre um banco ativo.

Persistem pendências de publicação: configuração e entrega real de e-mail, identificação formal do operador e contato oficial, retenção e exercício de direitos, hospedagem persistente, restauração real de backups e operação contínua de moderação e atendimento. Esta rodada não declara o site inteiro pronto para publicação.

