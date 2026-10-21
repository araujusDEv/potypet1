> Registro histórico. Para esta entrega, use Node 24.21.0 LTS e siga [ATUALIZACAO-ETAPA6.md](ATUALIZACAO-ETAPA6.md) e [PUBLICAR-HOJE.md](PUBLICAR-HOJE.md). Resultados abaixo pertencem à etapa indicada, não à conferência atual.

# Atualização para Appet 4.4.0 — 12/10/2026

Documento histórico preservado. Para a entrega 4.5.0, siga **ATUALIZACAO-ETAPA5.md** e **../RELATORIO-ETAPA5.md**. Os resultados e instruções atuais prevalecem sobre esta referência.

Esta entrega preserva as etapas 1, 2 e 3. Atualiza o carregamento, as fotos, a compatibilidade e a experiência das telas. Não recrie o banco nem importe o seed sobre dados existentes.

1. Pare o serviço e faça uma cópia consistente de **todo o DATA_DIR**: banco, eventuais WAL/SHM, arquivos privados, fila de e-mail, anexos de campanhas e backups. Guarde também as configurações e confira a possibilidade de restauração.
2. Extraia o ZIP em uma pasta separada. Substitua o código pelo conteúdo de `appet`, incluindo `js/app-runtime.js`, `lib/profile-media.cjs` e `lib/account-limits.cjs`. Preserve seu DATA_DIR; o ZIP contém apenas o seed original de demonstração, sem banco operacional. Se usa `appet/data` como armazenamento, preserve o conteúdo operacional dessa pasta antes da troca e antes de iniciar.
3. Preserve as variáveis reais, especialmente DATA_DIR, MAIL_QUEUE_KEY, PUBLIC_BASE_URL, SMTP, COOKIE_SECURE e credenciais administrativas. A chave da fila não deve ser trocada com mensagens pendentes. Em produção, use HTTPS, cookies seguros, disco persistente e contas/dados de demonstração desativados.
4. Execute `npm ci` na nova pasta de código, com Node.js compatível (verificado com 22.23.1), e inicie o serviço. Não há migração destrutiva nem necessidade de editar registros manualmente. `node_modules` é instalado à parte.
5. Confira login, animais e pedidos antigos, termos, acompanhamentos, campanhas, anexos e permissões. Teste busca, perfil, favoritos e revisão administrativa com contas separadas de homologação. As exigências de autenticação e confirmação de e-mail continuam valendo.
6. Configure PUBLIC_BASE_URL com a origem HTTPS real para o compartilhamento. O servidor entrega os metadados do perfil sem JavaScript e disponibiliza a imagem de preview somente para anúncios públicos. Serviços externos podem manter previews antigos em seus próprios caches; solicite nova leitura quando necessário.

## Fotos e limites

As fotos originais permanecem no banco ou nos arquivos existentes. Miniaturas WebP, versões para a galeria e previews JPEG são criados sob demanda em `DATA_DIR/derived-media`, com autorização verificada antes da resposta. Essa pasta é um cache regenerável; não substitui os originais. O cache tem alvo de 150 MiB, com limpeza periódica e possibilidade de excedente temporário durante processamento.

Limites padrão por conta:

| Conta | Anúncios de adoção não arquivados | Desaparecidos ativos | Fotos/anexos |
|---|---:|---:|---:|
| Usuário | 50 | 10 | 100 MiB |
| ONG | 300 | 100 | 500 MiB |
| Administração | 2.000 | 500 | 1.024 MiB |

Configure USER_ANIMAL_LIMIT, USER_MISSING_LIMIT e USER_STORAGE_MB; os prefixos ORG e ADMIN seguem o mesmo padrão. Os valores devem ser inteiros positivos. Adoção inclui anúncios não arquivados de qualquer estado; desaparecidos ativos são os não marcados como encontrados. Fotos de animais, desaparecidos e acompanhamentos e anexos de campanhas contam no armazenamento, inclusive registros arquivados. Imagens compartilhadas do pacote, textos, banco/índices, backups e cache derivado ficam fora dessa cota. O limite anterior de anexos por campanha continua existindo.

Registros acima dos novos limites são preservados. O servidor bloqueia acréscimos que excedam a cota, permitindo consulta, edição textual e redução das fotos. Arquivamento conserva o histórico e não libera o espaço das fotos antigas. Aceites anteriores dos termos não são alterados; novos aceites registram a versão desta etapa.

## Verificação e retorno à versão anterior

`npm test` executa 14 suítes isoladas; `npm run test:etapa4` executa os sete grupos específicos de API, mídia, cotas, compatibilidade e preservação. Veja `../RELATORIO-ETAPA4.md` e `../qa/LEIA-ME.md` para as verificações de navegador e desempenho. Não execute os testes contra o banco real.

Se houver falha, pare o serviço e restaure o código anterior junto com a cópia consistente dos dados e configurações. Confira os registros antes de reabrir o acesso.

Continuam pendentes o e-mail oficial e a entrega SMTP real, validação no domínio/hospedagem definitiva, identificação do operador, contato oficial e regras operacionais de retenção, atendimento e moderação. Os testes locais desta entrega não declaram o site inteiro pronto para publicação.

