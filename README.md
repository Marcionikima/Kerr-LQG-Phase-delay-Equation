# 🌌 LQC Phase-Channel Recovery Pipeline (Psi_Echo Model)

[![Python Version](https://shields.io)](https://python.org)
[![License: MIT](https://shields.io)](https://opensource.org)

Este repositório contém um pipeline estatístico automatizado para validação e recuperação do parâmetro quântico gama (γ) baseado no modelo **Psi_Echo** em Cosmologia Quântica em Laços (LQC). 

O pipeline simula o comportamento de ecos gravitacionais originados de perturbações no buraco negro central **Sgr A***, avaliando a capacidade de detecção do futuro observatório espacial **LISA** (Laser Interferometer Space Antenna).

⚠️ **AVISO IMPORTANTE:** Este projeto realiza testes estatísticos estritamente sobre **dados sintéticos**. Não há evidências observacionais diretas inseridas e **não existem dados reais do LISA ainda**, dado que a missão está planejada para a década de 2030.

---

## 📋 Sumário
- [Arquitetura do Modelo](#-arquitetura-do-modelo)
- [Funcionalidades do Pipeline](#-funcionalidades-do-pipeline)
- [Estrutura do Repositório](#-estrutura-do-repositório)
- [Pré-requisitos e Instalação](#-pré-requisitos-e-instalação)
- [Como Executar](#-como-executar)
- [Métricas Geradas](#-métricas-geradas)

---

## 🔬 Arquitetura do Modelo

O modelo teórico original decompõe a resposta do eco gravitacional em dois canais analíticos principais:

1. **Canal de Amplitude (Descartado):** O termo de refletividade possui um expoente de supressão extrema (~ -6.7 × 10⁻⁹¹ para Sgr A*). Numericamente, \(\exp(\text{expoente}) \equiv 1.0\) em qualquer precisão física mensurável. Portanto, este canal **não carrega informação útil** e foi removido do teste de recuperação.
2. **Canal de Fase (Utilizado):** O atraso temporal medido (\(\Delta_t\)) é sensível a γ de forma logarítmica:
   \[\Delta_t(\gamma, \theta) = A \cdot \ln\left(\frac{C}{\gamma}\right) \cdot f(\theta)\]
   Onde A representa o tempo de trânsito em Schwarzschild (~ 84.5 s), C é uma constante astrofísica de escala (~ 3 × 10⁹⁰) e f(θ) modela a inclinação angular com base no spin do buraco negro (\(a_* = 0.90\)).

---

## ⚙️ Funcionalidades do Pipeline

O script realiza as etapas completas de um pipeline de dados científicos:
* **Geração de Dados Sintéticos (Simulação):** Injeta um ruído gaussiano realista em segundos no atraso de fase (\(\sigma_t\)) e propaga o erro analiticamente para o espaço do parâmetro γ.
* **Análise Estatística Avançada:** Executa reamostragem via **Bootstrap** (1000 iterações) para extrair o Erro Padrão (SE) exato dos estimadores (Média e Mediana).
* **Validação de Regime Linear:** Avalia se o nível de ruído viola a aproximação linear de primeira ordem através do indicador adimensional \(\sigma_u\).
* **Geração de Relatório Executivo:** Exporta automaticamente um documento profissional em **PDF** contendo a tabela de resultados consolidados e os gráficos de dispersão das métricas.

---

## 📂 Estrutura do Repositório

```text
├── pipeline.py                 # Código-fonte principal (Simulação, Estatística e PDF)
├── README.md                   # Documentação do projeto (este arquivo)
<!-- Arquivos gerados após a execução: -->
├── pipeline_chart.png          # Gráfico de distribuição gerado pelo Matplotlib
└── Relatorio_Psi_Echo_LQC.pdf  # Relatório final formatado para apresentação
```

---

## 🛠️ Pré-requisitos e Instalação

O projeto foi desenvolvido em Python 3.8 ou superior. Para instalar as dependências necessárias, execute:

```bash
pip install numpy pandas matplotlib reportlab
```

### Detalhes das Dependências:
* `numpy` & `pandas`: Processamento numérico vetorial e manipulação de dataframes.
* `matplotlib`: Renderização dos gráficos de distribuição de frequência.
* `reportlab`: Construção programática do relatório PDF estruturado.

---

## 🚀 Como Executar

Para rodar todo o pipeline (executar as simulações, gerar os gráficos e construir o relatório em PDF), utilize o comando:

```bash
python pipeline.py
```

O terminal exibirá o progresso das simulações e confirmará a exportação dos artefatos:
```text
Iniciando Pipeline de Análise Estatística Psi_Echo...
Sucesso: Relatório PDF salvo como 'Relatorio_Psi_Echo_LQC.pdf'
```

---

## 📊 Métricas Geradas

O pipeline avalia a robustez da recuperação sob quatro cenários padrão de ruído de temporização ($\sigma_t$ de $1\text{s}$ a $100\text{s}$):

* **Viés da Média / Mediana (`bias_mean` / `bias_median`):** Mede o desvio sistemático do estimador em relação ao valor real injetado ($\gamma = 0.2375$).
* **Chi² por Grau de Liberdade (`chi2_dof`):** Avalia a qualidade do ajuste de ruído no espaço observável.
* **Critério de Regime Linear:** Indica se a propagação de erro de primeira ordem permanece confiável ou se o ruído colapsa a inversão matemática.
