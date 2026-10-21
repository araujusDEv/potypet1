# Atualização — PotyPet 4.7.0-rc.3

Entrega intermediária de 13/10/2026. Corrige a espera, confirmação, atualização dos dados e mensagens das ações administrativas. A final 4.7.0 depende da revisão final. Código recebido: ZIP PotyPet 4.7.0-rc.2, SHA-256 `39ab193b4d92a56cabd26bdc969b8289d6902fd0c79163759e5168c5ec523d5e`, conferido com a entrega anterior. Origem em `../RECEBIMENTO.json`; resultados em `../RELATORIO-ADMIN.md`.

## Preservar dados na atualização

1. Pare a aplicação e faça backup consistente de todo `DATA_DIR`, incluindo `adotapet.db`, arquivos WAL/SHM que existirem, fotos, anexos e históricos. Preserve as variáveis de ambiente e as chaves da fila de e-mail e dos backups, fora do código.
2. Extraia o ZIP em uma pasta nova. Substitua somente os arquivos de código de `appet/`, incluindo `admin.html`, `js/pages/admin.js`, package/lock, testes e documentação. Preserve o diretório de dados ativo inteiro. Se usa o padrão local `appet/data`, não extraia o código sobre essa pasta de dados nem reimporte o seed.
3. Mantenha o mesmo `DATA_DIR`, banco, IDs, cookies, credenciais, endereços e chaves. Nenhum código de servidor ou esquema do banco foi alterado nesta etapa; não há migração manual, recriação de conta ou banco. Amostras permanecem desligadas no ambiente real.
4. Use Node 24.21.0 e `npm ci`. Execute `npm test` em uma cópia sem dados operacionais: 19 suítes, incluindo `test:concurrency` (19 regressões anteriores) e `test:admin` (39 casos novos dos scripts). `npm run check:package` deve ser executado no pacote de código, sem banco/configurações privadas.
5. Reinicie com os dados e a configuração preservados. O endereço do script administrativo tem uma nova versão de cache; mantenha o novo `admin.html`. Confira login, histórico e uma ação autorizada de administração: o botão deve permanecer ocupado até terminar, a tela deve mostrar o estado confirmado e uma falha deve apresentar seu erro.

## Como interpretar os avisos

Erros da operação (403/409/503 etc.) não produzem sucesso nem alteram a tela como se a ação tivesse sido aceita. Quando a alteração recebeu confirmação, mas uma leitura ou o registro adicional de histórico falhou, a interface mantém o registro confirmado e mostra essa falha específica, sem sucesso genérico. Essa situação não autoriza reenviar uma operação destrutiva para tentar corrigir o aviso.

As verificações foram locais, em dados fictícios. Não houve publicação nem alteração de domínio/hospedagem. Testes de e-mail não comprovam SMTP real; persistência no provedor, pagamentos, aparelho físico/leitor de tela e carga de produção não foram validados. As pendências operacionais e a revisão final continuam; elas não impedem seguir para novas correções sobre esta rc.3.
