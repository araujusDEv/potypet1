# Atualização de marca — PotyPet 4.7.0-rc.1

Esta é uma **entrega intermediária para revisão**, baseada no projeto Node.js/SQLite do ZIP Appet etapa 6, de 12/10/2026. A versão final **4.7.0** será entregue somente depois da revisão final. PotyPet é o nome público desta etapa e das próximas. Não houve publicação, alteração de domínio ou alteração da configuração de hospedagem.

## Substituir somente o código

1. Pare o serviço e qualquer instância que use o banco. Guarde uma cópia consistente e todo o `DATA_DIR`, incluindo banco, arquivos personalizados e históricos. Preserve as variáveis de ambiente, `MAIL_QUEUE_KEY` e `BACKUP_ENCRYPTION_KEY` fora do pacote.
2. Extraia o ZIP em uma pasta separada. A pasta **appet/** continua sendo a pasta do código por compatibilidade. Substitua somente os arquivos de código; não substitua o banco, uploads, backups, diretório dos dados ou configurações. Se usa `appet/data` no ambiente local, preserve o conteúdo operacional antes da troca; o ZIP inclui apenas `data/seed.json`.
3. Use Node 24.21.0 LTS e execute `npm ci` pelo lockfile. Inclua os arquivos ocultos, scripts, bibliotecas e a pasta `docs/historico/`. Execute `npm test` em homologação, com dados fictícios e bases isoladas, antes de reiniciar o serviço oficial.
4. Reinicie com as variáveis existentes. Não altere `DATA_DIR`, endereço público, remetente real, serviço Render, cookies, chaves ou domínio por causa da marca. O JavaScript/CSS tem nova versão de cache; recarregue o navegador se ainda aparecer a interface antiga.
5. Confira login de conta existente, retorno ao perfil, publicação/aprovação de animal, campanhas e consulta de contratos, conversas e históricos. O novo logotipo deve aparecer como **PotyPet**, com o mesmo símbolo e cores.

## Documentos e registros antigos

A atualização não percorre o banco substituindo palavras. Contratos, notificações, mensagens privadas, auditorias, dados de contas, arquivos, fotos e evidências anteriores permanecem como registrados.

Novos termos de adoção recebem `brandName=PotyPet` e o aviso do documento no momento da conclusão. Esses campos ficam registrados para manter a identidade daquele documento em atualizações futuras. Termos antigos sem esses campos continuam sendo apresentados com Appet no documento; cabeçalho e menu do site usam PotyPet. Concordâncias, códigos `ADT-...`, cláusulas e datas não são refeitos.

Os textos anteriores de termos de uso e privacidade estão preservados, byte a byte, em `docs/historico/Appet-4.6.0/`. São arquivos de referência do pacote, sem exposição pelo servidor web. As páginas atuais mostram PotyPet e a identificação `2026-10-13-potypet-rc1`; contas existentes mantêm o aceite anterior. Um cadastro iniciado na versão anterior continua exigindo senha e link válidos e conserva a versão aceita antes da troca.

Novos e-mails usam PotyPet no assunto, remetente nominal e textos que identificam a plataforma. O endereço de envio configurado não é alterado. E-mails já enfileirados continuam criptografados com o conteúdo antigo; não são reescritos. Não troque a chave da fila.

Novos cartazes exibem PotyPet. PDFs/cartazes já exportados ou arquivos históricos externos não são modificados. O código não reescreve fotos ou documentos enviados.

## Identificadores preservados

Leia [ORIGEM-E-COMPATIBILIDADE.md](ORIGEM-E-COMPATIBILIDADE.md). Entre os preservados estão `adotapet.db`, cookie `petadopt_session`, preferências `petadopt_*`, retorno `appet-auth-return`, origem técnica `adoptionOrigin=appet`, nome interno npm `appet`, códigos `APPET-...` de atendimento, `ADT-...`, caminhos de dados e backup, cabeçalho criptográfico `APPET-BACKUP-1`, rotas, IDs, parâmetros e variáveis de ambiente. Isso conserva sessões, preferências, referências de protocolo, indicadores e restauração de cópias antigas.

`render.yaml`, `render.demo.yaml`, `.env.production.example`, `.node-version` e `proxy.nginx.example.conf` mantêm os arquivos anteriores. Nenhum recurso foi recriado ou contratado. O nome do workflow mudou para PotyPet; seus passos/runtime permanecem iguais.

## Verificações e limites

Consulte `../RELATORIO-POTYPET.md` e `../qa/execution-summary.json`. Os testes usam bancos fictícios isolados; o ensaio de atualização parte da versão Appet 4.6.0 e reutiliza seus dados, sessões e chave da fila. E-mail usa transporte file local. A conferência não comprova entrega SMTP, armazenamento no provedor ou recebimento bancário.

A publicação com dados reais continua dependendo das verificações operacionais descritas em [PUBLICAR-HOJE.md](PUBLICAR-HOJE.md). Esta entrega não é a aprovação final 4.7.0 nem uma autorização para publicar.
