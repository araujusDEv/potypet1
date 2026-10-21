> Registro histórico. Para esta entrega, use Node 24.21.0 LTS e siga [ATUALIZACAO-ETAPA6.md](ATUALIZACAO-ETAPA6.md) e [PUBLICAR-HOJE.md](PUBLICAR-HOJE.md). Resultados abaixo pertencem à etapa indicada, não à conferência atual.

# Atualização para Appet 4.5.0 — campanhas e confiança

Esta versão continua a etapa 4 e conserva seus arquivos, imagens, fluxos e dados. Não recrie o banco e não importe o seed sobre a instalação existente.

1. Pare o serviço. Faça uma cópia consistente de todo o DATA_DIR, incluindo banco e eventuais WAL/SHM, anexos privados, fila de e-mail e backups. Guarde as configurações e confirme a possibilidade de restauração.
2. Extraia o ZIP em uma pasta separada e substitua somente o código pelo conteúdo de `appet`. Preserve os dados operacionais. Se usa a pasta `appet/data` padrão, conserve seu conteúdo operacional antes da troca e antes de iniciar. O pacote contém somente o seed original, sem banco de produção.
3. Preserve DATA_DIR, MAIL_QUEUE_KEY, configurações de e-mail, PUBLIC_BASE_URL, HTTPS/cookies e credenciais administrativas. Não troque a chave da fila com mensagens pendentes. Execute `npm ci` com Node.js compatível (verificado com 22.23.1) e inicie o serviço.
4. O servidor adiciona metadados à tabela de evidências e aos registros de campanhas. Não remove os bytes dos arquivos existentes. Os documentos de decisões anteriores são conservados de forma preventiva quando o registro antigo não identifica quais foram usados. Verificações antigas continuam no histórico; novas aprovações exigem o registro por assunto.
5. Confira campanhas antigas, documentos privados, Pix ativo, histórico de alterações, termos, adoções e acompanhamentos com contas de homologação separadas. A confirmação de e-mail e a aprovação de organizações continuam obrigatórias conforme a configuração existente.

## Regras da nova revisão

Na área administrativa, confira identidade do organizador, finalidade, custos, recebedor/chave e evidências. Se houver prestação de contas, confira-a também. Registre o método, o resultado, os arquivos usados e a fonte independente quando houver. O sistema guarda administrador, data, ciclo, versão proposta e referências aos arquivos, com hash dos bytes armazenados. O resumo público exige confirmação de revisão de privacidade e rejeita formatos óbvios de contatos, documentos e links privados. Isso não substitui a revisão humana do texto.

Formato legível não comprova identidade, titularidade bancária, autenticidade do documento ou veracidade do caso. O sistema não promete detectar fotos de IA ou falsificações com certeza. Pessoas, clínicas, fornecedores e banco precisam confirmar o que estiver fora do alcance da análise documental. Fontes, notas detalhadas e documentos ficam privados; a capa autorizada é a exceção pública.

Chave, meta, textos e valores propostos aguardam revisão enquanto a versão aprovada continua pública. Valores são declarados manualmente, sem confirmação bancária automática. Atualizações financeiras exigem data real da apuração, descrição das despesas ou ausência delas e explicação dos valores/saldo. Não há conciliação automática ou leitura de extratos.

Pausar ou encerrar impede gerar novos Pix no servidor. Atualizar uma campanha pausada mantém a pausa até reativação administrativa explícita. Encerrar não permite reabrir o Pix, mas aceita prestações de contas posteriores. Se a administração encerrar enquanto existe proposta pendente, preserva essa proposta no histórico e mantém a publicação aprovada. Chaves e códigos já copiados continuam fora do controle do Appet: o banco pode aceitar transferência direta; o bloqueio do site não revoga a chave bancária.

## Inatividade e evidências

- Rascunhos não publicados sem atividade por 90 dias são arquivados, com dados e arquivos preservados. O responsável pode restaurá-los. Há até 100 campanhas não arquivadas e até 1.000 registros de campanha por conta.
- Após 30 dias sem prestação de contas atualizada, a campanha recebe aviso público e notificação interna. Aos 60 dias, o recebimento é pausado no servidor. Reativar exige aprovação de prestação de contas recente. Uma atualização apenas de título ou cuidados não reinicia esse prazo.
- Os prazos são avaliados pelo servidor nas consultas/operações de campanhas, incluindo a geração de Pix. Não é necessário um agendador para aplicar o bloqueio ao solicitar novo código. Não há envio real de e-mail comprovado para esses avisos; são notificações internas.
- A referência é a data de apuração aprovada; sem ela, usa-se a publicação conhecida. Não são inventadas datas de apuração para campanhas antigas. Na primeira consulta após atualizar, registros antigos já vencidos podem receber aviso ou pausa.
- Evidências sem uso em decisões podem ser removidas. As utilizadas podem ser retiradas de próximas análises, mantendo a cópia privada e referências das decisões. Para corrigir, retire a anterior e envie uma nova, sem sobrescrever o arquivo utilizado.
- A capa aprovada exige outra versão aprovada, substituindo ou retirando a capa, antes de retirar seu arquivo. Campanhas em análise não permitem alteração das evidências. Até 12 evidências ativas por campanha; as retiradas e preservadas não ocupam uma vaga ativa, mas continuam no armazenamento.
- Continuam as cotas de fotos/anexos por conta e o limite existente de 50 MiB de evidências de campanhas por responsável. Arquivamento ou retirada com preservação não libera bytes desses documentos. Não há eliminação automática dos registros usados em decisões; o operador ainda deve definir retenção e atendimento a solicitações de direitos.

## Verificação e restauração

`npm test` executa 15 suítes; `npm run test:etapa5` executa os testes específicos desta rodada em base temporária. O executor de navegador e os resultados estão em `../qa/`, com instruções próprias. Não execute esses testes apontando para produção.

Em caso de falha, pare o serviço e restaure o código anterior e a cópia consistente dos dados/configurações. Confira os registros antes de reabrir. Não alterne versões sobre o banco ativo.

Continuam pendentes e-mail oficial/SMTP real, validação na hospedagem definitiva, identificação do operador, retenção, restauração real de backups e operação humana de moderação e atendimento. Esta entrega não declara o site inteiro pronto para publicação.

