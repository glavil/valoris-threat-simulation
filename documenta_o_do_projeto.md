# Valoris Financial Platform — Simulação de Ameaças & Avaliação de Maturidade (MITRE ATT&CK)

> **Domínio:** Setor Financeiro / Fintech & Banking  
> **Arquitetura do Ambiente:** Híbrida (Windows Active Directory On-Premises + AWS Cloud & SaaS)  
> **Framework de Segurança:** MITRE ATT&CK® Enterprise (v20)  
> **Status do Projeto:** Ativo / Avaliação de Gaps Defensivos

---

## 🏢 1. Perfil Corporativo e Contexto da Empresa

A **Valoris Financial Platform** é uma instituição financeira de médio porte, nativa digital (fintech/banco digital), especializada em soluções de crédito automatizado, pagamentos instantâneos, gestão de contas corporativas (PJ/PF) e plataformas de investimento. 

Por atuar diretamente no setor financeiro, a Valoris lida diariamente com transações de altíssimo volume, dados sensíveis de clientes regulados por leis de privacidade e rígidas exigências de conformidade com órgãos reguladores (como o Banco Central e regulamentações de proteção de dados). 

### 📐 Dimensão e Operação
* **Colaboradores:** Aproximadamente 1.200 funcionários (entre operações, engenharia de software, suporte e atendimento financeiro).
* **Infraestrutura de Identidade:** Baseada primariamente em **Windows Active Directory (AD)** para estações de trabalho corporativas (Windows 10/11) e servidores locais.
* **Infraestrutura de Negócio (Nuvem):** Microsserviços e APIs bancárias hospedadas na **Amazon Web Services (AWS)** e ambientes SaaS integrados.

---

## 🎯 2. Metodologia de Avaliação de Maturidade Defensiva

Para identificar lacunas (*gaps*) de segurança e mensurar a capacidade de resposta contra grupos de ameaças persistentes avançadas (APTs como FIN7, APT29, etc.), a Valoris mapeou seu posture assessment utilizando as **14 táticas do MITRE ATT&CK Enterprise**.

A nossa postura defensiva não é tratada como um cenário binário (seguro vs. vulnerável). Em vez disso, utilizamos uma escala de pontuação de maturidade com gradiente visual customizado no **ATT&CK Navigator**:

| Score | Cor / Nível | Status Operacional | Definição do Nível na Valoris |
| :---: | :--- | :--- | :--- |
| **1** | 🟦 **Oportunidade de Melhoria / Ponto Cego** | *Vulnerável / Sem Telemetria* | Áreas que carecem de visibilidade, sem monitoramento ativo ou controles preventivos adequados, representando focos prioritários de hardening. |
| **2** | 🟨 **Detecção Parcial / Apenas Logs** | *Visibilidade Básica / Sem Alerta em Tempo Real* | O evento gera registros (Sysmon, Windows Event Logs, AWS CloudTrail), mas falta correlação automatizada no SIEM ou capacidade de bloqueio inline. |
| **3** | 🟩 **Detectado e Bloqueado / Maduro** | *Controle Preventivo Ativo* | Controles maduros e automatizados com EDR, WAF, MFA restrito e resposta ativa em tempo real sem latência humana. |

---

## 📊 3. Resumo Consolidado do Mapeamento (Valoris Corp)

Abaixo está o inventário técnico consolidado refletido no ficheiro de configuração `valoris_corp.json` do ATT&CK Navigator:

| Tática (MITRE ATT&CK) | ID da Técnica | Nome da Técnica | Score | Classificação / Comentário Operacional |
| :--- | :--- | :--- | :---: | :--- |
| **Reconnaissance** | `T1595` | Active Scanning | 🟡 2 | Scans externos na AWS geram logs no GuardDuty; perímetro legado sem correlação. |
| **Reconnaissance** | `T1589` | Gather Victim Identity Info | 🔴 1 | Ponto cego: Sem monitoramento de vazamentos de credenciais corporativas na Dark Web. |
| **Reconnaissance** | `T1596` | Search Open Technical Databases | 🔴 1 | Ausência de auditoria contínua em repositórios públicos de código e registros DNS. |
| **Resource Development** | `T1588` | Obtain Capabilities | 🟡 2 | Threat Intel consome feeds genéricos de IoCs, sem foco em TTPs de APTs financeiros. |
| **Resource Development** | `T1583` | Acquire Infrastructure | 🔴 1 | Sem monitoramento automatizado de domínios *Typosquatting* contra os clientes do banco. |
| **Initial Access** | `T1566` | Phishing (Spearphishing) | 🔴 1 | Anexos compactados com senha (`.zip`/`.rar`) passam pelo gateway sem inspeção profunda. |
| **Initial Access** | `T1190` | Exploit Public-Facing App | 🟡 2 | WAF ativo nas APIs modernas; endpoints e APIs legadas possuem janelas de patch longas. |
| **Initial Access** | `T1078` | Valid Accounts (Cloud/VPN) | 🔴 1 | Credenciais de acesso remoto sem análise comportamental de localização (*Impossible Travel*). |
| **Initial Access** | `T1195` | Supply Chain Compromise | 🔴 1 | Bibliotecas open-source (npm/pip) utilizadas em pipelines CI/CD sem análise de dependências. |
| **Execution** | `T1059` | Command and Scripting Interpreter | 🔴 1 | Logs de execução existem, mas comandos obfuscados em Base64 não disparam alertas no SOC. |
| **Execution** | `T1204` | User Execution (Malicious File) | 🟢 3 | **Protegido:** EDR barra automaticamente a execução de binários não assinados. |
| **Persistence** | `T1547` | Registry Run Keys / Startup | 🟢 3 | **Protegido:** Monitoramento ativo do EDR impede alterações não autorizadas no Registro. |
| **Persistence** | `T1053` | Scheduled Task / Cron | 🔴 1 | Servidores Linux de backend na nuvem sem agentes de EDR para auditar cron jobs maliciosos. |
| **Persistence / Privesc** | `T1098` | Account Manipulation | 🟡 2 | Alterações em grupos do Active Directory geram eventos, mas sem regras de risco imediato. |
| **Privilege Escalation** | `T1548` | Abuse Elevation Control (UAC) | 🟢 3 | **Protegido:** GPOs corporativas forçam UAC no nível máximo em todas as estações. |
| **Defense Impairment** | `T1222` | File and Directory Permissions Mod | 🔴 1 | Falta de monitoramento de integridade de arquivos (FIM) em diretórios de configuração crítica. |
| **Defense Evasion** | `T1036` | Masquerading | 🟢 3 | **Protegido:** Validação de assinatura digital de processos pelo EDR bloqueia renomeações. |
| **Defense Evasion** | `T1070` | Indicator Removal | 🟡 2 | Logs locais possuem janela de retenção antes do envio ao SIEM centralizado. |
| **Defense Evasion** | `T1027` | Obfuscated Files or Information | 🔴 1 | Empacotadores e binários altamente ofuscados conseguem burlar inspeções estáticas basais. |
| **Credential Access** | `T1003` | OS Credential Dumping (LSASS) | 🟡 2 | Bloqueia ferramentas clássicas (Mimikatz), mas falha em despejos via APIs nativas do Windows. |
| **Credential Access** | `T1649` | Steal/Forge Auth Certificates | 🟡 2 | Emissão de certificados no AD auditada, porém sem detecção comportamental de abusos. |
| **Credential Access** | `T1539` | Steal Web Session Cookie | 🔴 1 | Roubo de tokens de sessão de operadores financeiros por infostealers sem telemetria de browser. |
| **Credential Access** | `T1110` | Brute Force | 🟢 3 | **Protegido:** Bloqueio automático de contas após 5 tentativas incorretas no Active Directory. |
| **Discovery** | `T1087` | Account Discovery | 🟡 2 | Consultas LDAP e enumeração de contas ocorrem com visibilidade de log padrão. |
| **Lateral Movement** | `T1021` | Remote Services (SMB/RDP) | 🟡 2 | Tráfego administrativo remoto legítimo passa sem restrições baseadas em contexto de risco. |
| **Lateral Movement** | `T1570` | Lateral Tool Transfer | 🟢 3 | **Protegido:** Microsegmentação rigorosa de rede bloqueia transferência de ficheiros internos. |
| **Collection** | `T1005` | Data from Local System | 🟡 2 | Acesso a dados locais livre nas estações; bases corporativas protegidas por criptografia. |
| **Collection** | `T1114` | Email Collection | 🔴 1 | Extração de arquivos de caixas postais (`.pst`/`.ost`) sem inspeção de Prevenção de Perda (DLP). |
| **Command & Control** | `T1071` | Application Layer Protocol (HTTPS)| 🟡 2 | Tráfego cifrado de saída na porta 443 sem inspeção profunda de SSL/TLS (*SSL Decryption*). |
| **Command & Control** | `T1573` | Encrypted Channel | 🔴 1 | Túneis C2 direcionados a infraestruturas de nuvem públicas não catalogadas passam despercebidos. |
| **Command & Control** | `T1105` | Ingress Tool Transfer | 🟢 3 | **Protegido:** Módulo Web do EDR bloqueia downloads de utilitários maliciosos conhecidos. |
| **Exfiltration** | `T1048` | Exfiltration Over Alt Protocol | 🔴 1 | Tráfego de exfiltração via consultas DNS (`DNS Tunneling`) sem filtragem estrita nos resolvers. |
| **Impact** | `T1486` | Data Encrypted for Impact | 🟢 3 | **Protegido:** Módulo comportamental anti-ransomware ativo com restauro por cópias de sombra. |
| **Impact** | `T1490` | Inhibit System Recovery | 🟢 3 | **Protegido:** Tentativas de remoção de *Volume Shadow Copies* bloqueadas em tempo real. |
| **Impact** | `T1485` | Data Destruction | 🟡 2 | Exclusões na AWS geram alertas no CloudTrail, exigindo intervenção manual do SOC. |

---

## 🖼️ 4. Visualização no ATT&CK Navigator

Para explorar visualmente a matriz de maturidade da Valoris Corp:

1. Acesse o [ATT&CK Navigator oficial](https://mitre-attack.github.io/attack-navigator/).
2. No canto superior esquerdo, clique em **Open Existing Layer** e selecione **Upload from local**.
3. Escolha o arquivo **`valoris_corp.json`** localizado na raiz deste repositório.

> *[ Insira aqui o seu print do ATT&CK Navigator mostrando a matriz colorida da Valoris ]*

---

## 🚀 5. Plano de Ação e Próximos Passos (Roadmap de Segurança)

* **Fase 1 (Hardening de Identidade e Acesso):** Mitigar os pontos cegos em credenciais válidas (`T1078`) e roubo de cookies de sessão (`T1539`) com adoção de MFA resistente a phishing (FIDO2/WebAuthn).
* **Fase 2 (Segurança em Supply Chain):** Implementar ferramentas de Software Composition Analysis (SCA) nos pipelines de CI/CD para cobrir os riscos de cadeia de suprimentos (`T1195`).
* **Fase 3 (Visibilidade em Cloud e Servidores Linux):** Instalar agentes EDR/EDR-compatíveis nos servidores de backend Linux da AWS para eliminar os pontos cegos de tarefas agendadas e cron jobs (`T1053`).

---
*Documentação mantida pelo Time de Engenharia de Segurança e Red/Blue Team da Valoris Corp.*