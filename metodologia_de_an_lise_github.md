# 🔍 Metodologia de Análise de Risco e Threat Intelligence — Valoris Corp

Este documento detalha o fluxo analítico completo adotado para mapear, correlacionar e priorizar os riscos de segurança cibernética na **Valoris Corp**. A abordagem fundamenta-se em inteligência de ameaças reais (*Cyber Threat Intelligence* - CTI) direcionadas ao setor financeiro e na correlação prática utilizando o framework **MITRE ATT&CK®**.

---

## 📌 Visão Geral do Processo Analítico

O processo de avaliação foi dividido em **5 etapas sequenciais**, partindo da coleta de inteligência externa até o cruzamento técnico com a postura defensiva interna da instituição:

1. **Filtragem Georreferenciada e Setorial de Incidentes**
2. **Investigação Forense de Casos Reais (Atribuição)**
3. **Mapeamento Individual de TTPs por Threat Actor**
4. **Análise de Intersecção e Geração de Heatmap**
5. **Gap Analysis: Cruzamento de Maturidade e Priorização**

---

## 1. Contextualização e Filtragem de Ameaças Reais

O primeiro passo da análise consistiu em delimitar o perfil de risco da organização com base em incidentes reais reportados no mercado, evitando suposições genéricas.

* **Plataforma Utilizada:** VenariX (Threat Intelligence).
* **Parâmetros de Filtragem:**
  * **Região:** América do Sul *(devido ao perfil de operação e maior incidência de campanhas regionais contra a Valoris)*.
  * **Setor:** *Financials* (Instituições financeiras, fintechs, seguradoras e cooperativas de crédito).
  * **Janela Temporal:** Incidentes registrados entre `01/01/2026` e `31/10/2026`.
* **Resultado Obtido:** O cruzamento retornou **45 empresas do setor financeiro** que sofreram ataques cibernéticos ou vazamentos de dados no período, evidenciando os vetores de intrusão predominantes contra o segmento.

> 🖼️ **[Insira aqui o print dos filtros aplicados na plataforma VenariX]**  
> *Sugestão de arquivo: `![Filtros VenariX](./assets/01_filtros_venarix.png)`*

> 🖼️ **[Insira aqui o print da listagem dos 45 incidentes encontrados]**  
> *Sugestão de arquivo: `![Resultados Incidentes](./assets/02_resultados_incidentes.png)`*

---

## 2. Investigação Forense de Casos Reais (Estudo de Caso)

Para compreender a mecânica operacional dos ataques identificados, aprofundamos a investigação em incidentes específicos extraídos da base de dados.

* **Caso Analisado:** *H2 Financiamento Ltda.* (Setor Financeiro — Brasil).
* **Natureza do Incidente:** Ataque de *Ransomware* (Outubro de 2026).
* **Threat Actor Atribuído:** **INC Ransom**.
* **Evidências Coletadas:** A análise detalhou o perfil da vítima, o resumo da execução do ataque, o status de exposição de informações (*Data Breach Status: Observed*) e as provas documentais que confirmam o comprometimento da infraestrutura.

> 🖼️ **[Insira aqui o print dos detalhes do incidente da H2 Financiamento Ltda.]**  
> *Sugestão de arquivo: `![Detalhes do Incidente](./assets/03_estudo_caso_inc_ransom.png)`*

---

## 3. Mapeamento Individual de TTPs por Threat Actor

A partir dos principais grupos criminosos identificados atuando contra o setor financeiro na região — como **INC Ransom**, **Akira**, **Qilin** e **The Gentlemen** —, realizou-se o mapeamento isolado de suas Táticas, Técnicas e Procedimentos (**TTPs**) no **MITRE ATT&CK Navigator**.

* **Objetivo:** Isolar o *modus operandi* de cada adversário em camadas individuais para compreender quais técnicas específicas cada grupo emprega nas fases de Acesso Inicial, Execução, Persistência, Evasão e Impacto.

> 🖼️ **[Insira aqui o print das abas individuais dos Threat Actors no ATT&CK Navigator]**  
> *Sugestão de arquivo: `![Mapeamento Individual TTPs](./assets/04_mapeamento_individual_actors.png)`*

---

## 4. Análise de Intersecção e Heatmap de Ameaças

Com os perfis individuais estruturados, todas as camadas dos *threat actors* foram cruzadas e sobrepostas para gerar um **Mapa de Calor (Heatmap)** consolidado.

* **Metodologia de Intersecção:** Através da função de agregação de pontuações no ATT&CK Navigator, o sistema destacou por gradiente de cores as técnicas estatisticamente **mais utilizadas em comum** pelos grupos que atacam instituições financeiras.
* **Valor Estratégico:** Essa visão consolidada permitiu identificar os vetores de ataque dominantes e recorrentes no cenário atual, servindo como base empírica para testar a resiliência da Valoris Corp.

> 🖼️ **[Insira aqui o print do Heatmap consolidado dos Threat Actors]**  
> *Sugestão de arquivo: `![Heatmap Threat Actors](./assets/05_heatmap_ameacas.png)`*

---

## 5. Gap Analysis: Comparação de Maturidade e Priorização de Riscos

A etapa final consistiu em confrontar o mapa de calor das ameaças externas diretamente com a camada de postura defensiva interna da **Valoris Corp** (`valoris_corp.json`).

* **Cruzamento de Camadas:** Sobrepusemos a matriz de maturidade interna da Valoris (avaliada em scores de 1 a 3) sobre as técnicas mais exploradas pelos atacantes.
* **Identificação de Vulnerabilidades Críticas:** O cruzamento revelou com precisão cirúrgica quais técnicas altamente exploradas pelos cibercriminosos encontravam-se descobertas (**Score 1 - Pontos Cegos**) ou com visibilidade apenas passiva (**Score 2 - Oportunidades de Melhoria**) na infraestrutura da Valoris.
* **Escalonamento de Prioridades:** Com base nessas lacunas comprovadas, o time de segurança pôde estruturar um **Roadmap de Remediação orientado a risco**, direcionando esforços de Engenharia de Detecção e investimentos para as defesas mais críticas.

> 🖼️ **[Insira aqui o print da comparação final entre a Valoris Corp e o Heatmap de Ameaças]**  
> *Sugestão de arquivo: `![Gap Analysis Final](./assets/06_comparacao_maturidade_valoris.png)`*

---

## 📁 Como Reproduzir Esta Análise no GitHub

Para organizar este material no seu repositório:
1. Crie uma pasta chamada `assets` na raiz do seu repositório no GitHub.
2. Faça o upload dos seus *prints* dentro dessa pasta com os nomes sugeridos acima (ou atualize os caminhos no texto).
3. Adicione este arquivo (`METODOLOGIA_ANALISE.md`) na raiz do projeto e crie um link para ele no seu `README.md` principal.

---
*Documentação técnica elaborada para o portfólio de Segurança Cibernética & Threat Intelligence — Valoris Corp.*