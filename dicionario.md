# 📊 Dicionário de Dados - Projeto BanVic

Este documento registra a inspeção inicial das tabelas do banco de dados BanVic, realizada por meio de **SQL no DBeaver**. O objetivo foi identificar a estrutura das tabelas e pontos de atenção para as etapas de limpeza e modelagem dimensional.

---

## 🔍 1. Inspeção Visual das Tabelas

### 🏢 Tabela: agencias
| Nome da coluna | Tipo | Observação |
| :--- | :--- | :--- |
| **cod_agencia (PK)** | texto | Identificador numérico. |
| **nome** | texto | Em todas as linhas se repete o termo “Agência” antes do nome. |
| **endereco** | texto | Logradouro, número, Bairro, cidade e CEP. |
| **cidade** | texto | Nome da cidade. |
| **data_abertura** | texto | Formato “aaaa/mm/dd”. |
| **tipo_agencia** | texto | Categoria da agência. |

### 👤 Tabela: clientes
| Nome da coluna | Tipo | Observação |
| :--- | :--- | :--- |
| **cod_cliente (PK)** | texto | Identificador numérico. |
| **primeiro_nome** | texto | Nome do cliente. |
| **ultimo_nome** | texto | Sobrenome do cliente. |
| **email** | texto | Endereço de e-mail. |
| **tipo_cliente** | texto | PF (Pessoa Física?). |
| **data_inclusao** | texto | Timestamp (aaaa/mm/dd). É necessário tantos detalhes sobre a data da inclusão do cliente? É relevante esta informação, inclusive sobre fuso horário? |
| **cpfcnpj** | texto | Nome um pouco confuso; registrado de formas diferentes, alguns com pontos e traço, outros sem ponto e traço. |
| **endereco** | texto | Percebi endereços com bairro antes do número da residência. |
| **cep** | texto | Registros inconsistentes (com e sem traço). |

### 👥 Tabela: colaboradores
| Nome da coluna | Tipo | Observação |
| :--- | :--- | :--- |
| **cod_colaborador** | texto | Identificador numérico. |
| **primeiro nome** | texto | Nome do colaborador. |
| **data_nascimento** | texto | Formato “AAAA/MM/DD”. |
| **endereco** | texto | Difícil identificar se é bairro ou cidade logo de cara. |
| **cep** | texto | Observada falta de padronização. |

### 💳 Tabela: contas
| Nome da coluna | Tipo | Observação |
| :--- | :--- | :--- |
| **num_conta** | texto | Número da conta. |
| **cod_cliente (SK)** | texto | Relacionamento com a tabela clientes. |
| **cod_agencia (SK)** | texto | Relacionamento com a tabela agencias. |
| **tipo_conta** | texto | Categoria da conta. |
| **data_abertura** | texto | Timestamp com fuso horário. |
| **saldo_total** | texto | Sem padronização de casas decimais. |
| **saldo_disponivel** | texto | Sem padronização de casas decimais. |

### 📈 Tabela: propostas_credito
| Nome da coluna | Tipo | Observação |
| :--- | :--- | :--- |
| **cod_proposta (PK)** | texto | Identificador da proposta. |
| **data_entrada** | texto | Timestamp com fuso horário. |
| **valor_proposta** | texto | Verificar padronagem de casas decimais. |
| **status_proposta** | texto | Status da análise. |

### 💸 Tabela: transacoes
| Nome da coluna | Tipo | Observação |
| :--- | :--- | :--- |
| **cod_transacao (PK)** | texto | Verificar coincidências com tabela propostas crédito. |
| **num_conta (FK)** | texto | Conta de origem/destino. |
| **valor_transacao** | texto | Valor da movimentação. |

---

## 🛠️ 2. Roteiro de Qualidade de Dados (Data Quality)

Durante a inspeção, identifico a necessidade de realizar uma inspeção mais acurada, por meio seguintes comandos SQL para validar a saúde do banco:

1. **Checagem de Nulos**: Importante para garantir que IDs essenciais não estejam vazios.
```sql
SELECT count(*) AS total_linhas, count(coluna_importante) AS total_preenchido FROM nome_da_tabela;
Validação de Categorias: Entender as opções de segmentação (ex: tipo_conta).

SQL
SELECT DISTINCT nome_da_coluna_categoria FROM nome_da_tabela;

📋 3. Próximos Passos (Checklist)
[ ] Rodar SELECT * em todas as tabelas do esquema LIGHTHOUSE.

[ ] Identificar todas as Chaves Primárias (PK) para modelagem.

[ ] Mapear "sujeiras" (ex: nomes em caixa alta/baixa misturados).

[ ] Criar a dim_dates (Item 3 do desafio técnico).
