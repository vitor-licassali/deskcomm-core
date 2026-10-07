# Core público — perfil do fork

Este checkout foi criado diretamente de `https://github.com/melgarafael/DeskcommCRM.git`, na release oficial `v1.73.0`, commit `ab2288643c4147acb9b9cda2c768a46b3d771687`. Não foi copiado do repositório privado de homologação. A licença MIT e os avisos de copyright originais permanecem em `LICENSE`.

## Branches e publicação

- Repositório público proposto: `vitor-licassali/deskcomm-core`, como fork verdadeiro de `melgarafael/DeskcommCRM`.
- `main`: espelho do upstream; não recebe a adaptação operacional deste fork.
- `vyktus/stable-1.73`: branch proposta como padrão, baseada na release fixada pela operação e contendo apenas adaptações genéricas.
- Este documento descreve a preparação local. Não comprova criação do fork, publicação, execução de CI ou implantação.
- O remote `upstream` lê o projeto oficial e tem push bloqueado. Configure `origin` somente depois de criar e verificar o fork público correto. Nunca aponte este checkout ao HML privado.

O commit de upstream `main` consultado em 06/10/2026 foi `9e1be6d58b9d0babb2f482d0e25b96f78fde6526`; ele não substituiu a release de produção. Atualizar a release exige revisão de compatibilidade, migrations e lock da operação.

## Fronteira do conteúdo

Este repositório recebe aplicação, melhorias reutilizáveis, migrations genéricas e fixtures sintéticas. Prompts reais, telefones, preços, rotinas, documentos, backups e credenciais de clientes pertencem à operação privada ou ao banco/Storage da organização. Uma configuração que ainda não tenha suporte nativo permanece privada até existir um contrato próprio.

Não importe branches, tags, commits ou arquivos do HML para este fork por conveniência. Uma contribuição genérica deve ser reconstruída a partir de uma mudança revisada, sem conteúdo de cliente em código, fixtures, mensagens de commit ou descrição de PR.

## Actions neste perfil

| Workflow | Gatilho | Política |
|---|---|---|
| `ci.yml` | PR e execução manual | Preserva unit, lint, typecheck, cercas e invariantes de banco. Retirado push duplicado. |
| `e2e.yml` | Manual | Suíte original preservada; executar no SHA exato candidato antes de promover mudanças de UI/fluxo. |
| `perf.yml` | Manual | Build/tamanho preservados; executar para mudanças que afetam entrega. |
| `publish-image.yml` | PR, tag explícita e manual | Preserva sondas de boot e gates de imagens; retirado build/publicação em cada push de `main`. |
| `release.yml` | Manual | Sem corte automático de tag após merge. Fluxo upstream requer revisão antes de uso no fork. |
| `relogio.yml`, `vigia-de-colisao.yml` | Manual | Sem schedules herdados. Relógio de produção pertence ao scheduler da instalação. |
| `acolhida.yml` | Evento herdado de PR | Guarda de dono original preservada; não executa código de contribuidores no fork. |

Os corpos das suítes e os agregadores que rejeitam jobs cancelados/pulados foram preservados. A expectativa estrutural do teste do relógio foi adaptada à execução manual, sem desligar a validação HTTP.

E2E/performance manuais não podem ser tratados como verificações já executadas ou como gates automáticos de PR. Antes de promover uma revisão, registrar seus resultados no SHA exato. Não configurar esses checks como obrigatórios automáticos sem ajustar a política de execução.

Usar runners padrão hospedados pelo GitHub. Não ligar `EXECUTOR_PROPRIO` em VPS de produção nem associar credenciais reais a jobs de PR. Não usar Actions como agendador permanente da aplicação.

## Limites atuais

Nenhum teste foi executado durante esta preparação. As alterações de workflow foram revisadas estaticamente; as suítes originais continuam disponíveis.

O workflow de imagens e o fluxo de release ainda contêm contratos de distribuição do upstream, incluindo procedência de tags na `main`. Como a branch padrão proposta diverge de `main`, não publicar imagens nem cortar versões deste fork antes de revisar essa procedência e os nomes dos canais. A operação corrente continua apontando para suas imagens fixadas.

Ao sincronizar `main` com upstream, sua árvore também traz os workflows originais. Essa sincronização pode disparar Actions herdados; revise a política do repositório e evite atualização automática por mera rotina.

Para contribuir de volta, criar uma branch a partir de `upstream/main` e levar apenas o conserto genérico. Não incluir esta adaptação de CI/branches no PR oficial.
