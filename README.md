# ZeroTier Rules Creator

Interface web simples para criar, revisar e explicar regras de firewall do ZeroTier.

## Objetivo

Este projeto ajuda a montar regras de forma visual e reduzir erros de sintaxe antes de aplicar no controller da rede.

## Funcionalidades

- Criação de regras `accept` e `drop` com múltiplas condições.
- Suporte a filtros de:
  - `ethertype`
  - `ipsrc` e `ipdest` (CIDR IPv4 e IPv6)
  - `sport` e `dport`
  - `ipprotocol`
  - `ztsrc` e `ztdest`
  - `macsrc` e `macdest`
  - operadores de tag (`tdeq`, `tdne`, `tdgt`, `tdlt`, `tsgt`, `tslt`)
- Geração de definições de tags (`tag`, `id`, `enum`, `default`).
- Importação e análise de regras já existentes com explicação textual.

## Regras e validações implementadas

- Validação de CIDR para IPv4 e IPv6.
- Validação de portas no intervalo `1-65535`.
- Validação de MAC no formato `aa:bb:cc:dd:ee:ff`.
- Bloqueio visual de campos inválidos na interface.

## Como usar

1. Abra o arquivo `index.html` no navegador.
2. Use **Create New Rule** para montar regras de tráfego.
3. Use **Create Tag** para definir tags de política.
4. Use **Import Rules** e clique em **Analyze Imported Rules** para revisar regras existentes.
5. Copie o resultado para aplicação manual no ZeroTier Central/API.

## Referência oficial ZeroTier

- Documentação de regras: https://docs.zerotier.com/rules/
- Guia principal: https://docs.zerotier.com/

## Demo

- https://bytesec.pro/zero.html

## Próximos passos sugeridos

- Integração direta com a API do ZeroTier para aplicar regras no controller.
- Exportação/importação estruturada (JSON) para facilitar versionamento.
