# Atualização — PotyPet 4.7.0-rc.6

Base: rc.5, SHA-256 `1c144f19a40fd3787443d087cdb621790bc26b17d8d5a35a7543e70240cf38cd`, conferido com a entrega anterior. Entrega de revisão; final 4.7.0 somente após revisão final. Não houve publicação, contratação, migração real ou configuração de provedor.

1. Faça backup consistente de todo DATA_DIR, configurações e chaves. Pare a aplicação para substituir código de uma instalação; preserve o banco/fotos/históricos e não importe seed ou recrie contas.
2. Extraia em pasta nova e substitua somente `appet/`, incluindo os documentos, atendimento.html e scripts. Não copie node_modules entre plataformas. Use Node 24.21.0 e npm ci. Cotas, nomes técnicos, banco/cookies/rotas, fotos, dependências, regras de suspensão e fluxos anteriores permanecem.
3. Não há alteração de schema nesta rodada. Termos/privacidade atuais passam à versão **2026-10-13-potypet-rc6-revisao**; cópias exatas dos documentos rc.5 ficam em docs/historico/PotyPet-4.7.0-rc.5. São arquivos históricos do pacote e não ampliam acesso público de outras áreas.
4. Usuários existentes mantêm terms_version/terms_accepted_at originais. Abrir páginas, login, reinício e atualização não atribuem aceite novo. Confirmação pendente de cadastro antigo conserva versão/data apresentadas naquele cadastro. Novos cadastros de teste precisam marcar o aceite explícito da versão de revisão; não abra para usuários reais com pendências.
5. Novos anúncios/correções pendentes avisam a administração; decisões sobre anúncio, conta, ONG e desaparecido têm aviso interno existente e fila de e-mail genérico quando transporte está habilitado/destinatário confirmado. Avisos antigos/fila/contratos/mensagens não são reescritos nem reenviados por atualizar. Os links continuam sujeitos a login/autorização. Nenhum e-mail real foi enviado por esta entrega.
6. Complete PENDENCIAS-OPERACIONAIS.md e campos [A DEFINIR: ...], confira GUIA-HOSPEDAGEM.md com o serviço/plano escolhido e finalize os documentos antes de dados reais. No futuro, publicação de texto final deve ter versão própria e comunicação/aceite explícito conforme decisão aplicável; não sobrescreva aceites antigos.

`npm test` executa 24 suítes, incluindo testes de revisão/avisos/aceites. Em homologação use dados fictícios isolados; produção exige SMTP/HTTPS/cookies e não aceita transporte file. Render fica como opção; PUBLIC_BASE_URL torna o runtime independente desse provedor.

Leia ../RELATORIO-DOCUMENTOS.md para resultados/limitações. Credenciais de envio, persistência no provedor, resposta humana, políticas de conservação e recuperação externa real continuam pendentes. A suspensão é da conta, não bloqueio absoluto da pessoa fora dela.
