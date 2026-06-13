# 📊 Bitcoin Watcher: Pipeline de Dados & Dashboard no Databricks

Este projeto consiste em um pipeline de dados "ponta a ponta" (End-to-End) desenvolvido no **Databricks (Community Edition)**. A solução consome dados de APIs externas, realiza transformações e conversões cambiais em tempo real, armazena as informações estruturadas em uma camada **Delta Lake**, alimenta um dashboard analítico e conta com uma rotina de orquestração automatizada com alertas por e-mail.

---

## 🛠️ Arquitetura e Fluxo do Projeto

O fluxo de dados foi desenhado seguindo as melhores práticas de Engenharia de Dados:

```
[ API Coinbase ] ----------> [ Ingestão e Processamento ] ----------> [ Tabela Delta ] ----------> [ Databricks SQL ]
[ API CurrencyFreaks ] ----> [   (Conversão USD -> BRL)  ]            [  (Delta Lake)  ]          [    (Dashboard)   ]
                                                                             |
                                                                             v
                                                                    [ Databricks Jobs ]
                                                                 (Agendado: a cada 20 min)
                                                                             |
                                                                             v
                                                                  [ Alerta por E-mail ]
                                                                  (Notificação de Sucesso)
```

1. **Ingestão (Requests):** Consumo automatizado de duas APIs simultâneas:
   * **Coinbase API:** Fornece o preço atualizado do Bitcoin em Dólar (USD).
   * **CurrencyFreaks API:** Fornece a taxa de câmbio atualizada do Dólar para o Real (BRL).
2. **Transformação & Cálculo:** Cruzamento dos dados das duas fontes para calcular o valor real do Bitcoin em moeda local (BRL).
3. **Armazenamento (Delta Lake):** Os dados processados são salvos em formato ACID no Delta Lake, garantindo performance, versionamento e confiabilidade.
4. **Orquestração (Jobs):** Um fluxo de trabalho agendado para executar **a cada 20 minutos**.
5. **Observabilidade & Alertas:** A cada execução bem-sucedida do Job, o Databricks envia um e-mail de confirmação para o endereço configurado.
6. **Visualização:** Um dashboard interativo criado no Databricks SQL que consome os dados do Delta Lake para monitoramento de métricas.

---

## 🗂️ Estrutura do Repositório

Como o projeto foi integrado nativamente ao GitHub utilizando o **Databricks Repos**, a estrutura de arquivos gerada segue o padrão de notebooks exportados:

* `01_ingestion_and_transformation.py`: Código em Python (Spark/Pandas) que realiza os requests nas APIs, executa o cálculo de conversão cambial e persiste a informação na tabela Delta.
* `02_databricks_sql_queries.sql`: Consultas estruturadas utilizadas para modelar e extrair os dados que alimentam os componentes visuais do dashboard.
* `README.md`: Documentação oficial do projeto.

---

## 🚀 Tecnologias Utilizadas

* **Databricks** (Ambiente Unificado de Dados)
* **Python** (Bibliotecas `requests` e manipulação de dados)
* **Apache Spark / Delta Lake** (Armazenamento robusto e otimizado)
* **Databricks SQL** (Criação de queries e dashboards)
* **Databricks Workflows (Jobs)** (Agendamento e alertas por e-mail)
* **Git / GitHub** (Versionamento e CI/CD através de Repos)

---

## 🔧 Como Replicar este Projeto

1. **Configurar as credenciais de API:**
   * Obtenha sua chave de API gratuita no [CurrencyFreaks](https://currencyfreaks.com/).
   * A API da Coinbase não exige autenticação para dados públicos de cotação.
2. **Importar os Notebooks:**
   * Conecte este repositório no seu Databricks usando a aba **Repos**.
3. **Criar a Tabela Delta:**
   * Execute o notebook de ingestão para criar a primeira versão da tabela no seu catálogo de dados.
4. **Configurar o Job:**
   * Vá em **Workflows** -> **Create Job**.
   * Selecione o notebook principal.
   * Adicione um Schedule (Cron) para rodar a cada 20 minutos (`*/20 * * * *`).
   * Na seção **Notifications**, configure o envio de e-mail para o seu endereço em caso de **Success**.
5. **Montar o Dashboard:**
   * Vá na aba **SQL** -> **Dashboards** e crie os widgets baseados na tabela Delta gerada.

---
Desenvolvido por um apaixonado por Dados. Conecte-se comigo no [LinkedIn] https://www.linkedin.com/in/guilhermecamargosilva/!
