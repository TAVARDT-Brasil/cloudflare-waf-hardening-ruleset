# Cloudflare WAF Hardening Ruleset & Blueprint de Segurança de Borda

[![Licence MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Cloudflare WAF](https://img.shields.io/badge/Cloudflare-WAF%20Hardened-F38020.svg?logo=cloudflare)](https://cloudflare.com)
[![Language](https://img.shields.io/badge/Language-Português-green.svg)](README.md)
[![Diagnostics Suite](https://img.shields.io/badge/TAVARDT-Diagnostics-5b3cf5.svg)](https://tools.tavardt.com/br/analise-cabecalhos-seguranca/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Regras de segurança perimetral e mitigação de ameaças na borda para Cloudflare, em estrita conformidade com as exigências técnicas da LGPD (Lei nº 13.709/2018).

Desenvolvido e mantido pela equipe de engenharia de segurança perimetral da **TAVARDT**.

> [!IMPORTANT]
> **Arquitetura Universal & Agnóstica de Stack:** Este repositório **não é restrito ao WordPress**. Ele foi projetado para blindar **APIs RESTful, microsserviços modernos, SPAs (Next.js, React, Vue) e backends corporativos (Node.js, Python, Go, Laravel, Java)**. O bloqueio de sondas legadas de CMS atua como uma **camada de higiene perimetral ativa**, descartando o ruído cego de botnets na rede da Cloudflare (< 1ms) antes que qualquer requisição maliciosa consuma conexões TCP, memória ou CPU do seu servidor de origem.

***

### Matriz de Proteção Multi-Stack

| Camada de Defesa | Vetores Mitigados | Aplicações & Stacks Protegidas |
| :- | :- | :- |
| **Isolamento de Credenciais** | Bloqueio de `.env`, `.git`, `.sql`, `package.json`, `composer.json` | **APIs, Next.js, Node.js, Python, Go, Laravel, React** |
| **Mitigação de Reconhecimento** | Bloqueio de Nikto, SQLMap, Acunetix, Masscan, Gobuster, Nmap | **Qualquer web app, microsserviço ou gateway de API** |
| **Higiene de Verbos HTTP** | Rejeição de métodos não seguros (`TRACE`, `TRACK`, `DEBUG`) | **APIs RESTful e microsserviços de produção** |
| **Gating de Autenticação** | Desafio gerenciado em rotas sensíveis (`/admin`, `/wp-login.php`) | **Dashboards administrativos, ERPs web e consolas** |
| **Higiene de Ruído Cego** | Descarte na borda de sondas de CMS (`/xmlrpc.php`, trackbacks) | **Alívio imediato de carga de CPU em servidores de origem** |

***

## 1. Visão Geral da Arquitetura Perimetral

Este repositório fornece regras declarativas prontas para aplicação na camada de borda da Cloudflare, prevenindo incidentes comuns em aplicações corporativas e serviços críticos:

* **Isolamento de Arquivos Confidenciais:** Bloqueio perimetral imediato para requisições a arquivos `.env`, repositórios `.git`, arquivos `.sql`, dumps de banco de dados e arquivos de manifesto (`composer.json`, `package.json`).
* **Mitigação de Varreduras Automatizadas:** Intercepção em tempo real de scanners maliciosos conhecidos (Nikto, SQLMap, Acunetix, Masscan, Nmap, Gobuster, DirBuster).
* **Blindagem de CMS & Superfície de Ataque:** Bloqueio de abusos direcionados a `/xmlrpc.php`, `/wp-trackback.php` e vetores automatizados de amplificação.
* **Filtragem de Verbos HTTP Inseguros:** Rejeição no perímetro de métodos de depuração potencialmente abusivos (`TRACE`, `TRACK`, `DEBUG`).
* **Desafio Gerenciado em Painéis Administrativos:** Mitigação de força bruta em `/wp-login.php` e `/admin` através de desafios inteligentes da Cloudflare.

***

## 2. Diagnóstico Prévio de Segurança

Antes de aplicar qualquer alteração em ambientes de produção, avalie o perfil de cabeçalhos de segurança, certificados e postura de borda do seu domínio utilizando a suíte aberta da TAVARDT:

* **Auditoria de Cabeçalhos e SSL:** Análise de Cabeçalhos de Segurança da TAVARDT
* **Catálogo Global de Ferramentas de Diagnóstico:** **TAVARDT Tools Brasil**

***

## 3. Guia de Implementação Rápida

### Opção A: Importação Direta via Cloudflare API v4

Utilize o arquivo declarativo [`rules/ruleset.json`](rules/ruleset.json):

```bash
curl -X PUT "https://api.cloudflare.com/client/v4/zones/SUA_ZONE_ID/rulesets/phases/http_request_firewall_custom/entrypoint" \
     -H "Authorization: Bearer SEU_CLOUDFLARE_API_TOKEN" \
     -H "Content-Type: application/json" \
     -data @rules/ruleset.json
```

### Opção B: Provisionamento Declarativo com Terraform

Utilize a configuração IaC contida em [`terraform/main.tf`](terraform/main.tf):

```bash
cd terraform
terraform init
terraform plan -var="zone_id=SUA_ZONE_ID"
terraform apply -var="zone_id=SUA_ZONE_ID"
```

***

## 4. Consultoria Especializada & Resposta a Incidentes

Caso sua organização necessite de auditoria perimetral aprofundada, conformidade com a LGPD, proteção de agentes autônomos de inteligência artificial ou suporte de engenharia contínuo:

* **Portal Corporativo:** **TAVARDT**
* **Solicitação de Diagnóstico sob Medida:** [Atendimento Técnico Especializado](https://ag.tavardt.com/br/contato/)

***

## Links Úteis

* **Portal Corporativo:** https://ag.tavardt.com/br/
* **Suíte de Diagnósticos:** https://tools.tavardt.com/br/
* **Contato Técnico:** https://ag.tavardt.com/br/contato/

***

### Licença
Distribuído sob a Licença MIT. Consulte o arquivo [LICENSE](LICENSE) para obter detalhes completos.

***

© 2026 TAVARDT. Todos os direitos reservados.
