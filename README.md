**Data Engineering Challenge - ANS Pipeline (Medallion Architecture)**

Este repositório contém a resolução do desafio técnico para Engenheiro de Dados, focado no processamento de dados públicos da ANS (Agência Nacional de Saúde Suplementar). O objetivo foi construir um pipeline robusto seguindo a arquitetura Medallion dentro do ambiente Databricks.

**🚀 Arquitetura do Projeto**

O pipeline foi estruturado em três camadas principais para garantir a linhagem, qualidade e performance dos dados:

1. Bronze (Raw): Ingestão dos dados originais da ANS "as-is". O arquivo ZIP é baixado, descompactado e armazenado em formato Delta sem alterações estruturais.

2. Silver (Refined): Limpeza e padronização. Nesta etapa, realizamos a tipagem das colunas (Casts), remoção de espaços em branco (Trims) e normalização dos nomes das colunas.

3. Gold (Curated): Camada de consumo analítico. Criamos agregações estratégicas por Operadora, Faixa Etária e Município para otimizar as consultas de negócio.
   
**🛠️ Tecnologias Utilizadas**

* Linguagem: Python & PySpark.

* Plataforma: Databricks (Serverless Compute).

* Armazenamento: Unity Catalog Volumes (para arquivos brutos) e Delta Lake (para tabelas).

* Gestão de Dados: Arquitetura Medallion.

**🧠 Desafios Técnicos & Soluções**

Durante o desenvolvimento no ambiente Databricks Serverless, foram superados dois obstáculos críticos de infraestrutura:

* Restrição de Rede (Egress): Implementação de uma lógica híbrida de download/upload via Network Connectivity Config (NCC), permitindo a ingestão via stream de dados para evitar bloqueios de DNS.

* Limite de gRPC (128MB): Para processar o arquivo CSV de ~276MB, contornamos o limite de mensagens do dbutils utilizando a API nativa de sistema de arquivos do Python diretamente nos Volumes do Unity Catalog, garantindo extração eficiente em blocos (chunks) sem estourar a memória.

**📂 Estrutura do Repositório**

* ans_bronze.ipynb: Script de ingestão, download e extração dos dados brutos.

* ans_silver.ipynb: Processamento de refinamento, tipagem e qualidade.

* ans_gold.ipynb: Agregações analíticas e respostas ao desafio.

* /prints: Evidências da execução e resultados das consultas.

**📊 Resultados das Consultas**

As análises respondem aos seguintes pontos solicitados:

1. Top 5 Operadoras: Identificação das empresas com maior volume de beneficiários ativos.

2. Perfil Demográfico: Faixa etária predominante na base de dados.

3. Distribuição Geográfica: Ranking decrescente de beneficiários por município.

**Como utilizar**

1. Configure um Unity Catalog Volume no seu Workspace Databricks.

2. Importe os notebooks na ordem numérica.

3. Ajuste as variáveis de catálogo e schema no notebook de configuração/bronze.
