# 📊 Desafio de Projeto: Processamento de Dados com Power BI e Microsoft Fabric

Este repositório contém a resolução do desafio de projeto focado em Processamento e Transformação de Dados, originalmente parte da formação Power BI Analyst. Como inovação técnica, os dados foram hospedados e processados utilizando o **Microsoft Fabric**, aproveitando as tecnologias de **Lakehouse** e **Dataflow Gen2**.

## 🚀 Tecnologias Utilizadas
*   **Microsoft Fabric**: Ambiente de engenharia de dados (Lakehouse).
*   **Dataflow Gen2**: Ferramenta de ETL (Power Query Online).
*   **Power BI Desktop**: Modelagem de dados e visualização.
*   **Python**: Script de conversão de dados SQL para CSV.

## 🛠️ Etapas de Transformação (ETL)

### 1. Ingestão de Dados
Os dados originais em script SQL foram convertidos para arquivos CSV e carregados na pasta **Files** do Lakehouse. Em seguida, foram promovidos a tabelas Delta (`employee`, `departament`, `project`, etc.) para garantir performance.

### 2. Limpeza e Refinamento
*   **Tratamento de Nulos**: Identificamos que o gestor máximo não possuía supervisor (`Super_ssn` nulo). O valor foi tratado para evitar erros de agregação.
*   **Padronização Monetária**: A coluna `Salary` foi convertida para o tipo **Número Decimal Fixo**.
*   **Divisão de Endereço**: A coluna original de endereço foi dividida em quatro novas colunas: `Número`, `Rua`, `Cidade` e `Estado`, permitindo análises geográficas mais granulares.
*   **Construção de Nomes**: Criamos a coluna `Nome_Completo` mesclando as colunas de primeiro nome, inicial e sobrenome.

### 3. Integração e Mesclagem
*   **Funcionários e Departamentos**: Realizamos o **Merge** entre as tabelas para associar o nome do departamento diretamente a cada funcionário.
*   **Hierarquia de Gestão**: Criamos a coluna `Nome_Gerente` através de uma auto-mesclagem na tabela `employee`, relacionando o `Super_ssn` ao `Ssn`.
*   **Combinação Única**: Mesclamos os nomes de departamentos com suas localizações (`Ex: Research - Houston`) para garantir que cada registro de localidade fosse único, facilitando a futura transição para o Modelo Estrela.

## 🧠 Justificativas Técnicas

### Por que Mesclar (Merge) e não Atribuir (Append)?
*   **Mesclar (Merge)**: Foi utilizado para enriquecer tabelas com informações de outras fontes (equivalente ao `JOIN` em SQL). É ideal quando queremos adicionar colunas lateralmente baseadas em uma chave comum.
*   **Atribuir (Append)**: Seria utilizado apenas se tivéssemos tabelas com a mesma estrutura que precisassem ser empilhadas (equivalente ao `UNION`). Como nossas tabelas possuem schemas diferentes, o Merge foi a única operação correta.

### Tratamento de Duplicidade em Departamentos
Durante a mesclagem de Departamentos e Localizações, identificamos que a expansão criava duplicatas na chave primária `Dnumber`. Para corrigir isso e permitir o relacionamento 1:N no Power BI, mantivemos a tabela de Departamentos original como dimensão única e tratamos a combinação de locais em uma consulta de referência separada.

---
*Este projeto demonstra a capacidade de transitar entre ambientes tradicionais (MySQL) e soluções modernas de nuvem (Microsoft Fabric).*
