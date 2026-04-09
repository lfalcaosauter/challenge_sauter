## Sistema de Recomendacao de Veiculos — Sauter Digital (MVP)

MVP de um motor de recomendacao que sugere veiculos similares de **marcas concorrentes**, resolvendo o problema de **Cold Start** em uma plataforma nova de classificados automotivos sem historico de navegacao dos usuarios.

## O Problema

A Sauter Digital, plataforma emergente de classificados de veiculos, tinha uma secao "Carros Similares" que exibia apenas veiculos identicos (mesma marca e modelo). Se o cliente nao encontrava o preco ou quilometragem ideal naquele modelo, abandonava o site.

## A Solucao

Uma secao **"Outros que voce pode gostar"** que usa **Recomendacao Baseada em Conteudo (Content-Based Filtering)** — a inteligencia vem das caracteristicas dos proprios veiculos (motor, preco, idade, combustivel, cambio), sem depender de dados comportamentais.

## Dados

Dataset da **Tabela FIPE** extraido do Kaggle (`vagnerbessa/average-car-prices-bazil`), com 290.275 registros filtrados para **24.031 veiculos unicos** (Janeiro/2022, deduplicados).

Colunas utilizadas: `brand`, `model`, `fuel`, `gear`, `engine_size`, `year_model`, `avg_price_brl`, `age_years`.

## Arquitetura do Notebook

O projeto esta todo em `mvp_recomendador_automotivo.ipynb`, dividido em:

### 1. Ingestao e Limpeza
- Filtro temporal (Janeiro/2022) para evitar duplicidade mensal
- Remocao de colunas sem valor preditivo (`fipe_code`, `authentication`, metadados temporais)
- Deduplicacao por combinacao de marca + modelo + cambio + combustivel + motor + idade
- Criacao de chave de busca textual (`termo_busca`) para pesquisa livre

### 2. Feature Engineering
- **Variaveis numericas** (`engine_size`, `age_years`, `avg_price_brl`): normalizadas com **RobustScaler** (resistente a outliers — precos variam de R$1.831 a R$7.695.250)
- **Variaveis categoricas** (`fuel`, `gear`): codificadas via **One-Hot Encoding** com `drop='if_binary'`
- `year_model` excluido por ser linearmente dependente de `age_years`

### 3. Motor de Recomendacao
- **Similaridade de Cosseno** como metrica — mede a direcao dos vetores (perfil do veiculo), nao a magnitude. Combinada com RobustScaler, forma dupla camada de protecao contra distorcoes de escala
- **Priorizacao de marcas concorrentes:** preenche o carrossel com outras marcas primeiro; completa com mesma marca se necessario (evita carrossel vazio em nichos)
- **Coerencia de preco:** margem de +-25% sobre o valor do carro original
- **Ranking:** Top N resultados ordenados por score de similaridade

### 4. Interface Interativa
- Busca flexivel por termos livres (ex: "corolla 2020", "toyota 1.8")
- Selecao numerada do veiculo exato
- Painel visual HTML com os dados do carro e tabela estilizada das recomendacoes

### 5. Bateria de Testes
- **10 testes automatizados** cobrindo diferentes segmentos: hatch popular, sedan medio, sedan premium, pickup diesel, carro antigo, esportivo exotico, ultra-barato, ultra-luxo e nicho militar
- **Teste de estresse** com 9 pares extremos comparados diretamente (sem filtros de negocio), validando que o motor diferencia desde carros quase identicos (score >95%) ate veiculos de universos opostos (ex: Fiat Uno 1.5 1995 R$4k vs Toyota Hilux 2.8 2023 diesel R$235k)

## Como Executar

### Pre-requisitos
- Python 3.10+
- Arquivo `fipe_2022.csv` na raiz do projeto

### Instalacao

```bash
python -m venv venv
source venv/bin/activate  # Windows: .\venv\Scripts\activate
pip install -r requirements.txt
```

### Uso

1. Abra `mvp_recomendador_automotivo.ipynb` no VS Code ou Jupyter
2. Execute todas as celulas (**Run All**)
3. Na secao 4, digite marca/modelo/ano (ex: "Civic 2015") e selecione o veiculo
4. As secoes 5 e 5.1 rodam automaticamente com os testes de validacao

## Tecnologias

| Biblioteca | Uso |
|---|---|
| **Pandas** | Manipulacao e limpeza de dados |
| **Scikit-Learn** | RobustScaler, OneHotEncoder, Cosine Similarity |
| **IPython** | Interface interativa com HTML estilizado |
