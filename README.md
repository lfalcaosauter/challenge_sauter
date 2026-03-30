## Sistema de Recomendação Automotivo - Sauter Digital
Este repositório contém o MVP de um motor de recomendação desenvolvido para a plataforma Sauter Digital. O objetivo principal é mitigar o problema de Cold Start e aumentar a retenção de usuários, sugerindo veículos de marcas concorrentes com especificações técnicas e faixas de preço similares ao interesse original do cliente.

# 📌 Contexto do Projeto
A plataforma identificou que usuários muitas vezes abandonam o portal por falta de opções comparativas. O novo sistema de recomendação "Others you may like" foi projetado para atuar como um consultor virtual, apresentando alternativas de mercado que o usuário talvez não tenha considerado.

# 🛠️ Solução Técnica
1. Pipeline de Dados e Feature Engineering
Filtro Temporal: O dataset foi restrito ao mês de Janeiro de 2022 para garantir a estabilidade dos preços e evitar modelos duplicados.

Tratamento de Dados: Limpeza de colunas irrelevantes e criação da feature age_years (Idade do Veículo).

Encoding & Scaling: Variáveis categóricas convertidas via One-Hot Encoding e variáveis numéricas (Preço e Motor) normalizadas com StandardScaler para evitar vieses de grandeza.

Busca Semântica: Implementação de uma chave de busca combinada (brand + model + year) para facilitar a localização do veículo alvo.

2. O Motor Matemático
Utilizamos a Similaridade de Cosseno (Cosine Similarity). Esta métrica calcula o "ângulo" entre os vetores de características dos carros. Quanto mais próximo de 1.0, mais tecnicamente semelhantes são os veículos.

3. Regras de Negócio (Filtros de Saída)
O algoritmo não entrega apenas o que é parecido, ele aplica filtros estratégicos para o negócio:

Regra de Ouro (Diversificação): Bloqueio automático de sugestões da mesma marca do veículo pesquisado, forçando a apresentação de concorrentes.

Coerência de Preço: Trava lógica de ±25% sobre o valor do carro original, garantindo que a recomendação seja financeiramente viável para o perfil do cliente.

Ranking: Exibição qualificada do Top 10 resultados.

## 🚀 Como Executar
1. Pré-requisitos
Python 3.12+

Arquivo fipe_2022.csv na raiz do projeto.

2. Instalação
Clone o repositório e instale as dependências:

Bash
# Criar ambiente virtual
python -m venv venv
source venv/bin/activate  # No Windows: .\venv\Scripts\activate

# Instalar bibliotecas
pip install -r requirements.txt
3. Uso do Sistema
Abra o arquivo projeto_recomendacao.ipynb no VS Code ou Jupyter:

Execute todas as células (Run All).

Localize a última célula: uma caixa de texto aparecerá no topo do VS Code.

Digite a marca ou modelo (Ex: "Civic 2015" ou "Toyota") e pressione Enter.

O sistema identificará o veículo e gerará a tabela comparativa instantaneamente.

# 🧪 Estratégia de Validação
A validação foi realizada via Testes de Cenário Lógico.

Exemplo: Ao buscar um "Toyota Corolla", o sistema valida com sucesso a entrega de modelos como "Honda Civic" ou "Chevrolet Cruze", respeitando rigorosamente a similaridade técnica e as travas de preço.

# 🧰 Tecnologias Utilizadas
Python 3.x

Pandas: Manipulação de dados.

Scikit-Learn: Escalonamento, Encoding e Cálculo de Similaridade.

NumPy: Operações matriciais.

Dica para o arquivo requirements.txt
Não esqueça de criar este arquivo na mesma pasta com o seguinte conteúdo:

Plaintext
pandas
scikit-learn
numpy