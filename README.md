# Análise Forense de Logs - Laboratório Desec

Investigação de servidor comprometido utilizando análise de logs no terminal do Kali Linux.

## Objetivo

Realizar uma investigação Blue Team em um servidor Linux que sofreu acesso não autorizado. O atacante conseguiu quebrar a confidencialidade ao acessar um portal restrito apenas para funcionários.

O principal desafio era identificar:
- Endereço IP do atacante
- Janela temporal do ataque (início e fim)
- Ferramentas utilizadas pelo atacante
- Arquivos e informações sensíveis acessados

## Cenário

Um servidor corporativo foi comprometido. O atacante obteve acesso a um portal interno restrito. A empresa solicitou uma investigação urgente para entender como o ataque ocorreu e quais dados foram expostos.

> "Precisamos da sua ajuda com a investigação, não fazemos ideia de como alguém conseguiu acessar nosso portal. Existe como rastrear?"  
> — E-mail do Diretor

## Ferramentas Utilizadas

- **Kali Linux** (terminal apenas)
- Comandos nativos do Linux (`cat`, `grep`, `awk`, `sort`, `uniq`, `wc`, `tail`, `head`, etc.)

**Nenhuma ferramenta GUI ou ELK Stack foi utilizada.**

## Logs Analisados

- `access.log` — Log de acesso do servidor web
- `lab.log` — Log complementar do laboratório

## Metodologia de Investigação

A análise foi realizada exclusivamente via terminal com os seguintes passos:

1. Exploração inicial dos logs
2. Identificação de IPs suspeitos
3. Filtragem por status HTTP 200 (sucesso)
4. Busca por acessos a recursos sensíveis (`db.sql`, `intranet`, `.env`, `passwd`, `.php`, etc.)
5. Análise temporal dos eventos
6. Identificação de ferramentas utilizadas (`wget` e `curl`)

### Principais Comandos Utilizados

```bash
# Visualizar o log
cat access.log | less

# Contar total de linhas
wc -l access.log

# Filtrar IPs únicos
awk '{print $1}' access.log | sort | uniq -c | sort -nr

# Buscar acessos com status 200
grep ' 200 ' access.log

# Procurar acessos a arquivos sensíveis
grep -E '\.(sql|env|passwd|php|intranet)' access.log

# Filtrar atividade de um IP específico
grep "IP_DO_ATACANTE" access.log
