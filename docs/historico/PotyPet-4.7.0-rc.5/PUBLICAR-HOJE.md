> Entrega intermediária para revisão. Base recebida: PotyPet 4.7.0-rc.4, hash conferido. A versão final 4.7.0 depende da revisão final. Consulte [ATUALIZACAO-FOTOS.md](ATUALIZACAO-FOTOS.md) e [ORIGEM-E-COMPATIBILIDADE.md](ORIGEM-E-COMPATIBILIDADE.md).

# Publicar o PotyPet 4.7.0-rc.4 — guia de conferência

**Decisão desta entrega: ainda bloqueado para operação pública com dados reais.** O código está preparado para revisão; falta comprovar e-mail, disco/proxy do provedor, cópia externa e responsáveis pela operação. Não houve contratação, publicação, envio SMTP real ou pagamento.

## 1. Preparar o ambiente

Use **Node.js 24.21.0 LTS**, conferido em [fonte oficial](https://nodejs.org/en/blog/release/v24.21.0). `.node-version`, `package.json`, scripts e GitHub Actions estão alinhados. Execute `npm ci`, `npm test` e `npm run check:package`; a CI ainda precisa rodar no GitHub/Linux antes de aprovar o ambiente de destino.

Coloque somente `appet/` na raiz do repositório, com os arquivos ocultos. O `render.yaml` é um modelo para revisão: plano `1c-2g`, uma instância, disco de 5 GB em `/var/data`, `DATA_DIR=/var/data/appet`, `PERSISTENT_DISK_PATH=/var/data`, HTTPS/cookies seguros, amostras desligadas e publicação automática **desativada**. Confirme disponibilidade, espaço e custo no painel antes de qualquer contratação. [Planos e Blueprint](https://render.com/docs/blueprint-spec), [Node no Render](https://render.com/docs/node-version).

Somente o conteúdo do ponto montado persiste; o disco é acessível ao serviço em execução, não ao build. Esse modelo usa uma instância e admite interrupção nas republicações. Cadastre uma conta de teste e foto autorizada, reinicie e republique, então confirme que ambos continuam acessíveis. Confira o usuário do processo e permissões do diretório/banco. A gravação local não substitui essa prova. [Discos persistentes](https://render.com/docs/disks).

## 2. Preencher segredos e e-mail

Configure no painel `.env.production.example` como referência — o arquivo não é carregado automaticamente. Em instalação nova, preencha os dois e-mails e senhas iniciais distintos (`ADMIN_*`, `DONOR_*`). `MAIL_FROM`, `SMTP_HOST`, `SMTP_USER` e `SMTP_PASSWORD` dependem do remetente oficial que você ainda vai criar e autorizar. Nenhum endereço pessoal foi adotado como atendimento oficial.

Use `EMAIL_TRANSPORT=smtp`, porta 465/TLS ou 587/STARTTLS obrigatório. Mantenha `COOKIE_SECURE=1`, `SEED_DEMO_ACCOUNTS=0` e `SEED_SAMPLE_DATA=0`. `PUBLIC_BASE_URL` deve ser somente a origem HTTPS oficial; no Render, `RENDER_EXTERNAL_URL` é a alternativa automática. Links não usam o Host fornecido pelo visitante.

Gere uma **MAIL_QUEUE_KEY** de 32 bytes aleatórios em base64 apenas em instalação nova. Na atualização, preserve exatamente a existente. Guarde-a fora do repositório e também fora do disco. Para gerar chaves no computador confiável: `node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"`. O Blueprint não gira essa chave automaticamente.

`npm run check:email` verifica conexão/TLS/autenticação, **não entrega em caixa postal**. Depois teste cadastro, reenvio, confirmação e recuperação em endereços externos controlados; confira caixa de entrada e spam. Confirmação dura 24 horas e exige a senha escolhida. Recuperação dura 30 minutos, é de uso único, revoga sessões anteriores e não entra automaticamente. A ONG ainda precisa de aprovação administrativa. Os testes desta entrega usaram mensagens locais e transporte simulado.

## 3. Conferir IP e atendimento

Mantenha `TRUST_PROXY=0` até mapear os proxies de entrada e a forma de preencher `X-Forwarded-For`. Isso ignora headers falsificados, mas atrás de proxy vários visitantes podem compartilhar limites. Só habilite `TRUST_PROXY=1` com `TRUSTED_PROXY_IPS` restrita e verificada, sem `/0`, e teste dois clientes distintos e tentativa de header falsificado. Endereços de saída do provedor não são prova dos proxies de entrada. [IPs de saída do Render](https://render.com/docs/outbound-ip-addresses).

Para infraestrutura própria, `proxy.nginx.example.conf` mostra um proxy controlado que sobrescreve o header e mantém o backend restrito à interface local. Substitua domínio/certificados; não copie os IPs desse exemplo para o Render.

Defina quem responde aos protocolos em **Administração → Denúncias/mensagens**, quem modera e quem recebe pedidos de privacidade. Crie e teste o canal oficial antes de adicioná-lo ao contato e à política. Confira fotos autorizadas, recebedores das campanhas e fontes da rede de apoio; use o diagnóstico administrativo para revisar exemplos sem apagar registros reais. Não prometa assinatura certificada, autenticidade de documentos ou recebimento bancário automático.

## 4. Backups e restauração

O modelo usa `BACKUP_AUTO=1`, `BACKUP_KEEP=3`, `BACKUP_MAX_MB=2048` e `BACKUP_MIN_FREE_MB=256`. O serviço verifica a necessidade de uma cópia diária a cada hora enquanto está ativo. Ele produz SQLite consistente com teste de integridade, reserva espaço para cópia/criptografia e conserva ao menos as duas últimas cópias; quando faltar espaço, sinaliza a pendência. Cópias aguardando exportação não são apagadas. Ajuste disco, cotas e orçamento conforme o crescimento; os 5 GB não garantem capacidade indefinida.

Cópias no mesmo disco não cobrem perda do serviço. Configure bucket **privado**, região, credenciais restritas, prefixo e **BACKUP_ENCRYPTION_KEY** independente de 32 bytes em base64; veja as variáveis `BACKUP_S3_*` no exemplo. O upload usa AES-256-GCM e tenta novamente em falhas. A rotina não apaga objetos remotos: configure retenção/lifecycle, alertas e acesso de recuperação no serviço externo. Guarde as duas chaves fora do disco/bucket.

O banco inclui contas, fotos armazenadas nele, evidências, contratos e históricos. Configurações, segredos e arquivos personalizados fora do banco exigem cópia própria; o cache de imagens derivadas pode ser regenerado. Faça backup adicional de todo o `DATA_DIR` com o serviço parado antes de atualizar.

Ensaio externo obrigatório: baixe uma cópia `.enc`, defina a chave correta e execute `node scripts/decrypt-backup.cjs ARQUIVO.enc NOVO_BACKUP.db`; depois `node scripts/restore-check.cjs NOVO_BACKUP.db NOVO_DATA_DIR_VAZIO`. Inicie uma instância isolada com as mesmas chaves e confira contas, fotos e históricos. Os comandos não sobrescrevem destinos existentes. A restauração local passou; upload/download e recuperação em um bucket real permanecem pendentes.

## 5. Liberar somente após evidências

Guarde evidências da CI, recebimento dos e-mails, persistência após reinício/republicação, IP real, recuperação externa e responsáveis definidos. Confira `/api/health` e os fluxos de adoção/campanha no domínio real. Pix deve ter o recebedor conferido no banco; testar BR Code não prova pagamento. Use [ATUALIZACAO-POTYPET.md](ATUALIZACAO-POTYPET.md) para atualizar mantendo o banco. O piloto com dados reais depende das mesmas correções operacionais; homologação fictícia pode continuar localmente.

