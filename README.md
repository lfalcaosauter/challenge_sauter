<<<<<<< HEAD
# 🚗 Sistema de Recomendação Automotivo - Sauter Digital

Este projeto apresenta um MVP de um motor de recomendação desenvolvido para a plataforma **Sauter Digital**. O objetivo é aumentar a retenção de usuários sugerindo veículos de marcas concorrentes com características técnicas e preços equivalentes.

## 📌 Contexto e Problema
A plataforma identificou baixa retenção pois o sistema sugeria apenas modelos idênticos. O novo carrossel **"Others you may like"** foca em alternativas de mercado para manter o cliente engajado.

## 🛠️ Solução Técnica

### 1. Desafio do Cold Start
Como a plataforma não possui histórico prévio, utilizamos **Filtragem Baseada em Conteúdo (Content-Based Filtering)**, baseando-se exclusivamente nos atributos técnicos do dataset FIPE.

### 2. Engenharia de Atributos
* **Seleção:** Descartamos IDs internos e mantivemos atributos decisores (Motor, Câmbio, Preço, etc).
* **Escalonamento:** Utilizamos `StandardScaler` para normalizar o preço e o motor, evitando que a diferença de grandeza enviesasse o cálculo.
* **Encoding:** Variáveis categóricas foram convertidas via `One-Hot Encoding`.

### 3. O Motor Matemático
Utilizamos a **Similaridade de Cosseno (Cosine Similarity)**. Esta métrica foca na "direção" dos atributos, sendo ideal para identificar perfis de veículos similares.

### 4. Regras de Negócio
* **Diversificação de Marcas:** Filtro obrigatório para exibir apenas marcas diferentes da buscada.
* **Coerência de Preço:** Trava lógica de **±25%** sobre o valor do carro alvo.

## 🧪 Estratégia de Validação
Validação realizada via **Testes de Cenário Lógico**, garantindo que as saídas (ex: buscar Corolla e receber Civic) façam sentido comercial.

## 🚀 Como Executar
1. Certifique-se de ter o arquivo `fipe_2022.csv` na pasta.
2. Instale as dependências: `pip install pandas scikit-learn`.
3. Execute o notebook `projeto_recomendacao.ipynb`.
=======
# challenge_sauter
MVP de busca de automóveis 
>>>>>>> 420bcdb6987961a623b8797daa32ae876828076a
