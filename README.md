# ui-schema

Contrato seguro para superfícies administrativas declarativas do ThinkIac

Repositório: https://github.com/ThinkIac/ui-schema.

## Uso no ThinkIac

Este pacote foi extraído de `packages/ui-schema`. Preserve a identidade `ui-schema` do manifest ao instalá-lo em um workspace Lógos. Views, rotas, ferramentas e hooks são descobertos pelo runtime e dependem dos provedores correspondentes. Esta extração publica o código atual; não converte JavaScript para LógosType nem implementa instalação por perfil.

## Dependências e validação

Consulte `package.json` e os imports para as capacidades exigidas do host.

O pacote não declara scripts npm de build/teste. Os testes existentes acompanham o fonte; a composição completa requer o host ThinkIac.

## Segurança e dados locais

Credenciais devem ser configuradas no ambiente de destino. Não acompanham esta extração bancos, sessões, uploads, perfis pessoais, arquivos `.env`, caches ou backups. As exclusões foram registradas em relatório externo à publicação. Configure segredos pelo host, sem incluí-los em commits.

## Documentação anterior

# ui-schema

`ui-schema` descreve interfaces declarativas renderizadas por hosts como
Settings. O pacote consumidor fornece uma `surface` com versão, tipo, estado e
um conjunto fechado de ações; o host renderiza os controles e devolve somente
ações previstas pelo contrato.

Use schema para formulários e ações simples. Use uma view Svelte em iframe
quando houver navegação, estado visual complexo ou interação que não possa ser
expressa declarativamente.

O schema não concede autoridade. A rota ou tool do pacote deve autenticar o
chamador, autorizar a operação e validar os dados novamente. Segredos não devem
ser incorporados ao documento da superfície.

Veja [Settings](../settings/README.md) para declarar `settingsTab` e `keysTab`,
e o [mapa central](../../README.md#contratos-de-pacotes-e-interface) para as alternativas.

## Registros encontrados no código

Não foram encontrados registros declarativos nesse formato; consulte a entrada e os exports do pacote.

A lista é um inventário estático e pode incluir registros em testes; não garante disponibilidade sem os provedores do host.

Há imports relativos para diretórios superiores. A instalação isolada e a composição por perfil precisam resolver esses vínculos antes de remover a cópia da base.

