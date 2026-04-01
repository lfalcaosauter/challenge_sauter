## Sistema de Recomendação Automotivo — Sauter Digital (MVP)

Este repositório contém o MVP de um motor de recomendação desenvolvido para a Sauter Digital. O objetivo principal é mitigar o problema de **Cold Start** e aumentar a retenção de usuários, sugerindo veículos de marcas concorrentes com especificações técnicas e faixas de preço similares ao interesse original do cliente.

## Contexto do Projeto

Foi observado que usuários muitas vezes abandonam o portal por falta de opções comparativas. O novo sistema de recomendação **"Others you may like"** foi projetado para atuar como um consultor virtual, apresentando alternativas de mercado que o usuário talvez não tenha considerado.

## Solução Técnica

### 1. Pipeline de Dados e Feature Engineering

- **Filtro Temporal:** Dataset restrito ao mês de Janeiro de 2022 para garantir estabilidade de preços e evitar duplicidade mensal.
- **Deduplicação:** Remoção de veículos com características idênticas (marca, modelo, câmbio, combustível, motor e idade).
- **Tratamento de Dados:** Limpeza de colunas irrelevantes (`fipe_code`, `authentication`, metadados temporais) e seleção de features relevantes.
- **Encoding & Scaling:** Variáveis categóricas (`fuel`, `gear`) convertidas via One-Hot Encoding com `drop='if_binary'`. Variáveis numéricas (`engine_size`, `age_years`, `avg_price_brl`) normalizadas com **RobustScaler** para resistência a outliers de preço.
- **Busca Textual:** Chave de busca combinada (marca + modelo + ano + motor) para localização flexível do veículo.

### 2. O Motor Matemático

Utilizamos a **Similaridade de Cosseno** (Cosine Similarity). Esta métrica calcula o "ângulo" entre os vetores de características dos carros. Quanto mais próximo de 1.0, mais tecnicamente semelhantes são os veículos. Escolhida em vez da Distância Euclidiana por ser insensível à magnitude dos vetores, capturando o **perfil** do veículo.

### 3. Regras de Negócio (Filtros de Saída)

- **Priorização de Marcas Concorrentes:** O carrossel prioriza veículos de outras marcas, mas completa com a mesma marca quando não há concorrentes suficientes — garantindo que o carrossel nunca fique vazio.
- **Coerência de Preço:** Trava lógica de ±25% sobre o valor do carro original.
- **Ranking:** Exibição qualificada do Top 10 resultados ordenados por similaridade.

### 4. Validação

Bateria de **testes programáticos de cenário lógico** cobrindo diferentes segmentos de mercado (popular, médio, luxo, antigo). Cada cenário valida: priorização de marca, margem de preço, similaridade positiva, ausência de duplicatas, respeito ao limite de resultados e exclusão do próprio veículo.

## Como Executar

### Pré-requisitos

- Python 3.10+
- Arquivo `fipe_2022.csv` na raiz do projeto

### Instalação

```bash
# Criar ambiente virtual
python -m venv venv
source venv/bin/activate  # No Windows: .\venv\Scripts\activate

# Instalar bibliotecas
pip install -r requirements.txt
```

### Uso do Sistema

1. Abra o arquivo `mvp_recomendador_automotivo.ipynb` no VS Code ou Jupyter
2. Execute todas as células (**Run All**)
3. Na seção interativa, digite a marca ou modelo (Ex: "Civic 2015" ou "Toyota") e pressione Enter
4. O sistema identificará o veículo e gerará a tabela comparativa
5. A seção de validação roda automaticamente, exibindo os resultados dos testes

## Tecnologias Utilizadas

- **Python 3.x**
- **Pandas:** Manipulação e limpeza de dados
- **Scikit-Learn:** RobustScaler, OneHotEncoder e Cosine Similarity
- **NumPy:** Operações matriciais
