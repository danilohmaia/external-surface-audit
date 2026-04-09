# Audit Report Template

Use this structure for developer-facing reports.

## Title

`Auditoria Externa de Seguranca - <nome do sistema>`

## Metadata

- Data da verificacao
- Alvo
- Escopo observado

## Resumo Executivo

- O que foi testado
- Achados confirmados
- Gravidade geral
- Ponto mais critico

## Metodologia

- abordagem caixa-preta
- sem autenticacao
- fontes observadas: HTML, bundles, headers, APIs publicas, arquivos publicos

## Superficie Tecnica

- stack identificada
- bundles relevantes
- backend identificado
- tabelas, colecoes, endpoints ou funcoes relevantes observadas no cliente

## Achados Confirmados

Para cada achado:

### Nome do achado

- o que ficou exposto
- como foi confirmado
- impacto
- exemplo mascarado
- gravidade sugerida

## Itens Testados sem Confirmacao de Exposicao

- liste os alvos que responderam vazios, negados ou protegidos
- explique que isso nao prova seguranca total

## Riscos Inferidos

- itens ainda nao confirmados, mas sugeridos pelo bundle, rotas ou arquitetura

## Prioridades de Correcao

### Prioridade 1

- acoes imediatas de contencao

### Prioridade 2

- revisao de policy, grants, authz e fluxo

### Prioridade 3

- hardening e monitoramento

## Checklist Tecnico

- RLS
- grants
- views
- functions
- storage
- headers
- logs
- rotacao de tokens

## Conclusao

- sintese curta
- recomendacao de repeticao do teste apos correcao
