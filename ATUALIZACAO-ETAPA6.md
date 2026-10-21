# Atualização segura — Appet 4.6.0 / etapa 6

Este procedimento substitui o código e mantém os dados. Os guias anteriores permanecem como histórico; para runtime, publicação e backups, siga esta etapa. Nunca use `data/seed.json` como substituto do seu banco.

## Antes da troca

1. Identifique o `DATA_DIR` real e as variáveis da instalação. Guarde **MAIL_QUEUE_KEY**, **BACKUP_ENCRYPTION_KEY**, credenciais SMTP/S3, domínio, configuração de proxy e demais segredos em local privado separado. Não coloque essas informações no ZIP ou repositório.
2. Pare o serviço e qualquer instância que use o banco. Preserve todo o `DATA_DIR`, incluindo `adotapet.db`, WAL/SHM existentes, arquivos personalizados e backups. Não copie somente o arquivo principal de um banco ativo.
3. Produza também uma cópia consistente com `node scripts/backup.cjs CAMINHO_NOVO.db`, utilizando o `DATA_DIR` correto. Verifique com `node scripts/restore-check.cjs CAMINHO_NOVO.db` e guarde uma cópia fora do serviço. Retenha a versão antiga do código para recuperação.
4. Se o diretório atual fica dentro do código ou em pasta temporária, copie-o, com o serviço parado, para um **novo diretório persistente externo ao código**. Preserve a origem. Ajuste `DATA_DIR` e `PERSISTENT_DISK_PATH` para o destino; confira permissões do usuário do processo. Em produção, localizações temporárias, dentro do código ou fora da montagem configurada são recusadas.
5. Revise contas com `admin123` / `doador123` antes de ativar produção: troque senhas pelas funções existentes em ambiente controlado sem apagar usuários ou vínculos. Se necessário, use [RECUPERAR_ADMIN.md](RECUPERAR_ADMIN.md) para o administrador e remova a variável de recuperação depois. Alterar `DONOR_PASSWORD` não troca a senha de um usuário já existente. Uma conta inicial ou uma foto de exemplo, isoladamente, não prova que o registro é fictício.

## Substituir e conferir

6. Instale **Node 24.21.0 LTS**. Use os arquivos de `appet/` da nova entrega e inclua `.node-version`, `.github/`, `.gitignore` e `.env.production.example`. Substitua somente o código; mantenha o banco fora dessa troca. Execute `npm ci` para instalar as dependências do lockfile no ambiente novo; não transporte `node_modules` da instalação antiga.
7. Mantenha as chaves criptográficas existentes. Em produção: SMTP real, origem HTTPS, cookies seguros, `SEED_SAMPLE_DATA=0`, `SEED_DEMO_ACCOUNTS=0`, disco montado e apenas uma instância. O Blueprint não é uma instrução para recriar ou substituir o serviço/disco atual. Não crie um disco vazio no lugar do existente.
8. Execute `npm test` em homologação com bases fictícias isoladas, depois inicie com o banco preservado. Migrações compatíveis são aplicadas na inicialização. Confira `/api/health`, acesso administrativo e **Conferir preparação**. Os testes não validam o banco real da sua hospedagem.
9. Consulte contas existentes, fotos originais, históricos, contratos e acompanhamentos; confira solicitações e campanhas, incluindo publicação aprovada e alterações pendentes. Se o envio foi ativado pela primeira vez, usuários comuns antigos precisam confirmar o endereço por reenvio ou recuperação. Administradores mantêm acesso gerenciado pelo operador. Não desative a confirmação para contornar falhas de entrega.
10. Revise os registros indicados como exemplos e as fotos antes de divulgar. O diagnóstico é somente leitura. Não remove, reimporta nem redefine os dados antigos. Depois confira a persistência após reiniciar e republicar no provedor e complete os testes reais descritos em [PUBLICAR-HOJE.md](PUBLICAR-HOJE.md).

## Recuperação sem sobrescrever o banco

Use uma cópia íntegra criada com `backup.cjs` ou descriptografada com `decrypt-backup.cjs`. Com o serviço parado, restaure em diretório **novo e vazio**:

```text
node scripts/restore-check.cjs CAMINHO_BACKUP.db NOVO_DATA_DIR_VAZIO
```

Aponte uma instância isolada para esse novo diretório usando as chaves preservadas. Confira contas, fotos, anexos e vínculos antes de apontar o serviço oficial ao destino. Preserve o banco anterior completo; não misture seus WAL/SHM com o arquivo restaurado. Nunca sobrescreva o banco ativo. Se voltar ao código anterior, utilize uma cópia anterior à atualização e o runtime/configuração compatíveis, mantendo o original guardado.

## O que foi efetivamente testado

Uma base fictícia foi criada na **etapa 5 / Node 22.23.1**, com contas confirmadas, foto enviada, adoção concluída, termos das duas partes, acompanhamento com foto/pedido de ajuda e campanha com evidências privadas e mudança pendente de Pix/valores. A mesma base foi aberta na **etapa 6 / Node 24.21.0**. Usuários, hashes/salts, coleções, documentos, sessões, tokens e IDs da fila foram preservados; conteúdo entregue da fila pode ser limpo normalmente. A publicação aprovada permaneceu separada do rascunho.

O backup foi restaurado localmente em diretório novo; a aplicação iniciou e os vínculos/fotos/histórico continuaram acessíveis. Essa verificação não comprova SMTP, persistência no Render, permissões Linux reais, capacidade com seu volume de dados ou recuperação de um bucket real.
