# Atualização — PotyPet 4.7.0-rc.5

Base: ZIP PotyPet 4.7.0-rc.4, SHA-256 `5ae44bb6745db0c69521403b136d6a21d949df87bfb5df6492dd41915ccd5659`, correspondente à entrega anterior. Esta versão permanece intermediária; a final 4.7.0 depende da revisão final. Não houve publicação, alteração de domínio/hospedagem ou banco operacional.

1. Pare a aplicação e faça backup consistente de todo `DATA_DIR`: banco, fotos, anexos, históricos e arquivos WAL/SHM existentes. Preserve configuração e chaves da fila de e-mail e dos backups. Confira a restauração numa cópia isolada.
2. Extraia em pasta nova e substitua somente o código de `appet/`, incluindo novas bibliotecas e `js/photo-upload.js`. Preserve o diretório de dados ativo inteiro, cookies, caminhos e variáveis. Não reimporte o seed nem crie outro banco.
3. Use o runtime compatível já adotado, Node 24.21.0 da série 24, e `npm ci`. Dependências e lock de dependências permanecem iguais; a versão do pacote passa a rc.5. Os scripts/cache do HTML devem ser atualizados juntos.
4. Não há migração de fotos, registros ou schema nesta rodada. As migrações anteriores continuam idempotentes. Fotos antigas, descrições vazias e decisões anteriores permanecem como estão; a nova regra atua somente numa nova decisão de aprovação, inclusive criação administrativa com aprovação.
5. Teste numa cópia: envio, edição sem novas fotos, aprovação/reprovação/exceção, acesso histórico restrito e reinício. `npm test` executa 23 suítes; `npm run test:photos` executa os dois conjuntos novos. Nunca use dados operacionais nos testes.

## Limites de fotos

Nos formulários de animal e desaparecido, novos originais JPG/PNG/WebP podem ter até **20 MB e 20 milhões de pixels**. O navegador corrige orientação, reduz o maior lado a **1280 px**, aplica fundo branco a transparência e comprime em JPEG antes de enviar. O limite de **3 MB é por arquivo já processado**. Até três fotos por animal; uma no desaparecido. Fotos inválidas, formato incorreto ou resultado ainda grande geram erro e mantêm os campos preenchidos.

O servidor não confia no navegador: confere base64/tamanho, assinatura/formato, imagem estática, limite de pixels, CRC de PNG e decodificação completa. Reorienta e reencoda novos bytes sem EXIF/GPS/XMP, além de aplicar autorização e cotas da conta. O corpo continua limitado a 13 MB. A cota de armazenamento considera o resultado que será salvo, sem inflar PNG a partir de um JPEG comprimido.

Somente fotos selecionadas novamente são processadas. Sem nova seleção, a edição omite `photos`/`photo` e conserva os bytes anteriores. Referências existentes a imagens empacotadas ficam compatíveis. Não se remove GPS de fotos antigas automaticamente; essa alteração exigiria revisão e autorização próprias. Dados visíveis dentro da foto (rostos, placas, documentos ou endereço) não são apagados pelo tratamento de metadados.

## Novas aprovações

Padrão: **uma foto tecnicamente válida e descrição não vazia**. Configure no ambiente:

```text
ANIMAL_APPROVAL_MIN_PHOTOS=1
ANIMAL_APPROVAL_MIN_DESCRIPTION_CHARS=1
```

O número de fotos pode ser 1, 2 ou 3. A descrição pode exigir de 1 a 2000 caracteres, sempre após remover espaços externos. Não há configuração para dispensar globalmente foto ou descrição; dispensas exigem exceção administrativa registrada na decisão. Valores inválidos impedem inicialização com mensagem clara. A configuração é lida no início e exibida nos formulários/painel; alteração exige reinício. Não muda decisões antigas.

No diálogo administrativo, **Registrar exceção administrativa à regra mínima** exige justificativa específica. A API também exige administrador atual e registra administrador/data/motivo/critério não atendido no histórico privado e no log. A justificativa geral da decisão continua obrigatória. Reprovar ou manter em análise não exige foto/descrição. Nenhuma exceção é concedida a contas comuns, e nenhum registro antigo é reprovado, ocultado ou apagado pelo mínimo novo.

A conferência técnica ocorre antes da leitura/gravação final, fora de transação. Sessão/papel são revalidados. Se fotos, descrição ou decisão mudam enquanto a imagem é processada, a aprovação recebe 409 e requer nova revisão, preservando o estado mais recente.

**Validade técnica não comprova autenticidade, saúde ou correspondência da foto com o animal e não detecta imagens de IA.** A revisão humana precisa conferir o animal e as fontes apropriadas. A marca de verificação registra revisão administrativa e não é certificação. Afinidade compara preferências declaradas; não prevê sucesso ou segurança.

## Indicadores e privacidade

O nome público passa a **Animais disponíveis com prioridade de atenção**: aprovados/disponíveis com necessidades especiais declaradas, idosos ou anunciados há mais de 60 dias. O campo técnico `urgent` permanece; isso não indica emergência veterinária.

Adoções concluídas contam pedidos `concluida`, incluindo histórico anterior preservado. No fluxo atual, conclusão exige entrega informada e recebimento confirmado; aprovação do interesse, concordância com termo e adoções fora do PotyPet não aumentam esse total. Registros da plataforma não comprovam independentemente a entrega.

Avisos nos campos explicam o que será público: contato, fotos, localização do desaparecimento, descrições e demais informações do anúncio. Titular/administração conservam acesso às decisões privadas. Suspensão, confirmação de e-mail, permissões e rotas anônimas seguem as correções anteriores. Não volte ao código anterior à rc.4 com novas suspensões, pois ele não aplica esse bloqueio.

Testes usam dados fictícios e confirmação por e-mail local. SMTP real, hospedagem, aparelho físico, Safari e leitor de tela não foram aprovados por esses testes. A rc.5 pode ser a base da próxima etapa.
