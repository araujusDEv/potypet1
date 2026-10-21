# Atualização, backup e recuperação — PotyPet 4.7.0

Base conferida: rc.6, SHA-256 53635e852f48b623603f049def803f8e2d63c8a162ccccf6b5bcb3b121fda089. Esta entrega não publica, migra uma instalação real ou recria banco. Os ensaios usam bases fictícias isoladas. Não há mudança de schema em relação à rc.6.

## Antes de substituir código

1. Confira o SHA-256 do ZIP e os hashes do MANIFESTO-SHA256.json. Extraia em uma pasta nova. Preserve a instalação anterior para poder voltar ao código anterior.
2. Identifique o DATA_DIR efetivo e guarde todas as configurações. Preserve MAIL_QUEUE_KEY, BACKUP_ENCRYPTION_KEY e as chaves correspondentes a backups antigos em local privado separado. Não envie essas chaves ao repositório, ZIP ou chat.
3. Gere snapshot consistente do SQLite com a rotina existente de backup. Não copie apenas adotapet.db enquanto ele estiver sendo gravado: WAL/SHM podem conter alterações. Confirme o teste de integridade do snapshot e copie para destino privado independente. Uma cópia no mesmo disco não protege contra perda do disco.
4. Preserve todo DATA_DIR e arquivos personalizados fora dele, além das imagens originais empacotadas. O banco contém contas, fotos/evidências, pedidos, termos, mensagens, históricos, decisões e fila. Configurações/segredos e arquivos externos ao banco precisam de cópia própria; miniaturas/cache podem ser reconstruídos.
5. Faça primeiro um ensaio com cópia fictícia ou autorizada, isolada da instalação ativa e sem notificações reais. Nunca prepare cenário escrevendo diretamente no banco operacional.

## Substituição do código

Pare o serviço antes de trocar os arquivos. Substitua somente o código de appet/; mantenha o DATA_DIR existente fora da pasta de código. Não copie data/seed.json sobre banco ou coleções. Não apague adotapet.db, não recrie contas e não habilite importação de amostras para atualizar.

Instale as dependências com npm ci usando Node >=24.21.0 <25 no SO/arquitetura do destino. Não copie node_modules Windows para Linux. Uma única instância e um único worker devem executar node server.js ou npm start. Preserve configurações de origem HTTPS, cookies, SMTP, proxy restrito, cotas, revisão e backups.

Em produção, SEED_SAMPLE_DATA=0 e SEED_DEMO_ACCOUNTS=0. Não substitua credenciais reais existentes por contas de demonstração. A aplicação verifica condições de produção; não remova confirmação de e-mail, exigências de HTTPS ou controles para contornar uma falha de configuração.

Inicie e confira autenticação explícita, fotos antigas, anúncios, pedidos e termos vinculados, históricos, suspensão/reativação, campanhas, fila e backups. Compare antes/depois e repita a conferência após reinício. npm test cria bancos de teste e não deve ser apontado para dados operacionais.

Os termos públicos continuam na versão de revisão 2026-10-13-potypet-rc6-revisao. Atualização, leitura, login e reinício não registram novo aceite. Versão/data de contas e confirmações pendentes anteriores permanecem. Finalizar os documentos exigirá identificar a nova versão e decidir comunicação/aceite explícito aplicável, sem reescrever recibos antigos.

## Recuperação em destino novo

Guarde uma cópia intacta do backup e das configurações. Use as chaves correspondentes. Quando o backup estiver criptografado, a ferramenta existente utiliza BACKUP_ENCRYPTION_KEY do ambiente:

~~~text
node scripts/decrypt-backup.cjs ARQUIVO.enc NOVO_BACKUP.db
node scripts/restore-check.cjs NOVO_BACKUP.db NOVO_DATA_DIR_VAZIO
~~~

Use caminhos novos e diretório vazio. Essas ferramentas não autorizam sobrescrever um banco ativo. Inicie uma aplicação isolada com os dados restaurados, o código/imagens necessários e a MAIL_QUEUE_KEY preservada; confira integridade, contas, fotos, vínculos, históricos, suspensões, contratos e leitura da fila. Teste download do backup externo e restauração, não apenas upload.

Antes de uma recuperação operacional, pare o serviço e faça revisão humana do destino e do resultado. Se uma atualização falhar, preserve o banco e logs; decida entre voltar ao código anterior ou restaurar em destino novo após conferência. Não restaure automaticamente uma cópia antiga sobre dados recentes. O orçamento, retenção, destino externo, responsáveis e objetivo de recuperação continuam em PENDENCIAS-OPERACIONAIS.md.

## Limites da conferência

Atualização rc.6 → 4.7.0, reinícios e backups locais são ensaiados com dados fictícios. Persistência no provedor, envio/recebimento SMTP, recuperação externa em serviço real e pagamento não foram certificados. Leia ../RELATORIO-FINAL.md.
