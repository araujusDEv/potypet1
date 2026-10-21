# Atualização — PotyPet 4.7.0-rc.2

Entrega intermediária de 13/10/2026. A final 4.7.0 depende da revisão final. Esta etapa corrige gravações após processamento de imagens; não altera esquema do banco, políticas, hospedagem, marca ou arquivos da interface.

## Origem conferida

Base recebida: `PotyPet-renomeacao-4.7.0-rc.1-2026-10-13.zip`, versão `4.7.0-rc.1`, 56.220.309 bytes. SHA-256: `48dc74544b673fda31ff3a2ff56b8377f1234ed90624f49447a7c5ae9acbfa19`. Confere com o hash da entrega PotyPet anterior. O hash do antigo Appet não foi exigido. Consulte `../RECEBIMENTO.json` e `../RELATORIO-CONCORRENCIA.md`.

## Substituir preservando os dados

1. Pare a aplicação para evitar gravações durante a cópia. Faça backup consistente do diretório completo indicado por `DATA_DIR`, incluindo `adotapet.db`, anexos, fotos, backups e demais dados. Com o servidor parado, preserve também eventuais arquivos `adotapet.db-wal` e `adotapet.db-shm`. Guarde separadamente a configuração de ambiente e as chaves da fila de e-mail e dos backups. Confira a possibilidade de restauração antes de trocar o código.
2. Extraia este ZIP em uma pasta nova. Substitua somente os arquivos de código de `appet/`, incluindo `server.js`, `lib/campaigns.cjs`, arquivos de dependências, testes e documentação. **Não substitua o diretório de dados ativo.** Se sua instalação usa o padrão local `appet/data`, preserve essa pasta inteira; não copie `data/seed.json` sobre uma base ativa nem extraia o ZIP por cima dela sem excluir os dados da operação de substituição.
3. Mantenha `DATA_DIR`, endereços, variáveis, credenciais e chaves existentes. O banco continua `adotapet.db`; cookies, sessões, IDs, protocolos e nomes internos não mudam. Esta correção não exige apagar tabelas, recriar contas, importar seed ou executar uma migração manual. Mantenha `SEED_SAMPLE_DATA=0` e `SEED_DEMO_ACCOUNTS=0` no ambiente real.
4. Use Node **24.21.0**, compatível com o projeto. Execute `npm ci` e `npm run check:package` na cópia do código sem dados operacionais. Execute `npm test` nessa cópia: 18 suítes usam bases fictícias isoladas; `npm run test:concurrency` executa os 19 casos novos. Não aponte testes para o banco real.
5. Inicie a aplicação apontando para o mesmo `DATA_DIR`. Confira login, fotos, anúncios e histórico. No registro de desaparecido ocultado, a correção do responsável deve conservar a ocultação e o motivo. Confira também os vínculos de pedidos, termos, acompanhamentos e campanhas. Nenhuma publicação ou mudança de domínio/hospedagem foi realizada nesta entrega.

## Limites

A correção evita novas perdas por esse padrão. Não reconstrói informações que já tenham sido perdidas na versão anterior; esses casos precisam ser conferidos com um backup e revisados antes de qualquer restauração de dados.

Nos campos enviados por duas edições, a última gravação autorizada prevalece. Campos não enviados, outros registros e metadados administrativos são preservados. Não foi acrescentado controle de versão ao formulário nem feita uma refatoração geral.

Os testes foram locais, em Windows/Node 24.21.0 e Chrome, com dados fictícios. Eles não comprovam SMTP real, persistência no provedor, pagamento ou prontidão geral para publicação. Consulte `PUBLICAR-HOJE.md` para as pendências operacionais anteriores.
