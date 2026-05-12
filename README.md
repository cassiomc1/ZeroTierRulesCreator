# ZeroTier Rules Creator

Interface web simples para criar, revisar e explicar regras de firewall do ZeroTier.

## Objetivo

Este projeto ajuda a montar regras de forma visual e reduzir erros de sintaxe antes de aplicar no controller da rede.

## Modos de Segurança

### 🟢 Modo Blacklist (Permissivo) — padrão

Todo tráfego é **permitido** por padrão. Você adiciona regras `drop` para bloquear tráfego específico. O conjunto de regras termina com um `accept;` incondicional.

Template gerado:
```
# BLACKLIST MODE: traffic is ALLOWED unless explicitly DROPPED
drop not ethertype arp and not ethertype ipv4 and not ethertype ipv6;
accept;
```

### 🔴 Modo Whitelist (Restritivo)

Todo tráfego é **bloqueado** por padrão. Você adiciona regras `accept` apenas para o tráfego que deseja permitir explicitamente. O conjunto de regras termina com um `drop;` incondicional.

Template gerado:
```
# WHITELIST MODE: traffic is BLOCKED unless explicitly ACCEPTED
accept ethertype arp;
accept ethertype ipv6;
accept ethertype ipv4;
drop;
```

> **Nota técnica:** No motor de regras do ZeroTier, se nenhuma regra corresponder ao tráfego e não houver ação incondicional, o comportamento padrão é DROP. O modo Whitelist torna isso explícito com um `drop;` final; o modo Blacklist torna tudo permitido com um `accept;` final. Fonte: `zerotier/ZeroTierOne include/ZeroTierOne.h` e `nonfree/controller/EmbeddedNetworkController.cpp`.

Ao usar o botão **Load Template**, a ferramenta carrega o baseline do modo selecionado. Ao adicionar regras via **Add Rule**, elas são inseridas automaticamente antes do terminador (`drop;` ou `accept;`) para manter a lógica correta.

## Funcionalidades

- Seletor de modo de rede: **Blacklist** (permissivo) e **Whitelist** (restritivo).
- Templates de baseline para cada modo, com inserção inteligente de novas regras.
- Criação de regras com ações `accept`, `drop`, `break`, `tee`, `watch`, `redirect` e `priority`.
- Suporte a filtros de:
  - `ethertype`
  - `ipsrc` e `ipdest` (CIDR IPv4 e IPv6)
  - `sport` e `dport`
  - `ipprotocol`
  - `ztsrc` e `ztdest`
  - `macsrc` e `macdest`
  - `vlan`, `vlanpcp`, `vlandei`
  - `iptos`, `icmp`, `framesize`, `random`, `chr`
  - operadores de tag (`teq`, `tdiff`, `tseq`, `treq`, `tand`, `tor`, `txor`) e compatibilidade com legados (`tdeq`, `tdne`, `tdgt`, `tdlt`, `tsgt`, `tslt`)
- Geração de definições de tags (`tag`, `id`, `enum`, `default`).
- Importação e análise de regras já existentes com explicação textual e validação com linha/coluna.
- Preset anti-spoof (`drop not chr ipauth;`) disponível em ambos os modos.

## Regras e validações implementadas

- Validação de CIDR para IPv4 e IPv6.
- Validação de portas no intervalo `0-65535` (uint16 conforme especificação ZeroTier), com suporte a ranges (ex: `80-443`).
- Ação `priority` validada como QoS bucket uint8 (`0-255`), conforme `ZT_VirtualNetworkRule` no ZeroTierOne.
- Validação de MAC no formato `aa:bb:cc:dd:ee:ff`.
- Validação de endereço ZeroTier no formato hexadecimal (10 caracteres).
- Validação de ranges e limites numéricos para campos avançados.
- Bloqueio visual de campos inválidos na interface.

## Como usar

1. Abra o arquivo `index.html` no navegador.
2. Use **Network Security Mode** para escolher entre Blacklist e Whitelist e carregar o template correspondente.
3. Use **Create New Rule** para montar regras de tráfego. Regras são inseridas automaticamente antes do terminador.
4. Use **Create Tag** para definir tags de política.
5. Use **Import Rules** e clique em **Analyze Imported Rules** para revisar e validar regras existentes.
6. Copie o resultado para aplicação manual no ZeroTier Central/API.

## Referência oficial ZeroTier

- Documentação de regras: https://docs.zerotier.com/rules/
- Guia principal: https://docs.zerotier.com/
- Código-fonte das regras (canônico): https://github.com/zerotier/ZeroTierOne/blob/dev/include/ZeroTierOne.h
- Compilador de regras de alto nível: https://github.com/zerotier/ZeroTierOne/tree/dev/rule-compiler

## Demo

- https://bytesec.pro/zero.html

## Próximos passos sugeridos

- Integração direta com a API do ZeroTier para aplicar regras no controller.
- Exportação/importação estruturada (JSON) para facilitar versionamento.
- Botões de preset de regras comuns por modo (ex: "Bloquear NetBIOS/SMB", "Permitir SSH").
