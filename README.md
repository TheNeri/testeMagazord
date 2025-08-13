# 📄 Planos de Testes 

Este documento descreve os **Planos de testes**, abrangência, ferramentas, execução e considerações finais para os cenários solicitados: integrações com marketplaces, sistemas de gestão de estoque e validação de dados cadastrais.

---

## 📑 Sumário

1. [Integração de Marketplaces Terceiros](#cenário-1-integração-de-marketplaces-terceiros)
2. [Integração com Bling (Gestão de Estoque)](#cenário-2-integração-com-bling-gestão-de-estoque)
3. [Erro de Anúncio – Pausado sem Estoque](#cenário-3-erro-de-anúncio--pausado-sem-estoque)
4. [Validação de Dados Cadastrais](#cenário-4-validação-de-dados-cadastrais)


---

## Cenário 1 Integração de Marketplaces Terceiros 

### 📌 Documentação e Materiais de Apoio

**Fontes utilizadas**:

* Documentação oficial das APIs (Amazon, Mercado Livre)
* Especificações técnicas da equipe de desenvolvimento
* Requisitos de negócio (estoque, precificação, frete)
* Histórico no Jira/Trello
* Diagramas de fluxo e arquitetura

**Análise**:

* Identificação de endpoints, campos obrigatórios e regras
* Checklist por módulo (estoque, anúncios, pedidos, faturamento, preços)
* Validação de requisitos funcionais e técnicos

**Mapeamento de requisitos**:

Exemplos:

| Requisito                | Caso de Teste                                    |
| ------------------------ | ------------------------------------------------ |
| REQ01: Atualizar estoque | CT01: Reduzir estoque após pedido confirmado     |
| REQ02: Criar anúncio     | CT02: Validar criação com título, imagem e preço |

**Ferramentas**:
Jira, Trello, Confluence, Google Docs, Postman, Swagger, Miro, IA

---

### 🎯 Abrangência dos Testes

**Funcionalidades**:

* Estoque, Anúncios, Faturamento, Pedidos, Preços

**Cases de Uso**:

* **Sucesso**: sincronização correta, pedido importado
* **Falha**: erro de autenticação, falha de sincronização
* **Carga**: alto volume simultâneo

**Exemplo de Priorização**:

| Cenário                        | Impacto | Frequência | Prioridade |
| ------------------------------ | ------- | ---------- | ---------- |
| Falha na importação de pedidos | Alta    | Alta       | Alta       |
| Erro de imagem em anúncio      | Baixo   | Média      | Média      |
| Atualização de preço           | Média   | Alta       | Alta       |

---

### 🛠 Execução dos Testes

**Ambiente**: homologação/stage, sandbox oficial, base anonimizada  
**Dados de teste**: produtos fictícios, pedidos simulados, usuários variados  
**Automação**: Postman, Selenium/Cypress, JMeter, CI/CD  
**Registro**: logs, notificações Slack/E-mail

---

### 📌 Considerações Finais

* Testes exploratórios
* Validação de resiliência
* Conformidade com LGPD

---

## Cenário 2 Integração com Bling (Gestão de Estoque) 

### 📌 Documentação e Materiais de Apoio

**Fontes**:

* Documentação oficial da API Bling
* Especificações técnicas
* Fluxos de integração
* Requisitos de negócio (estoque, pedidos, produtos)
* Histórico no Jira/Trello/ClickUp
* Registros de reuniões

**Mapeamento de requisitos**:
Exemplos:

| Requisito                | Caso de Teste                    |
| ------------------------ | -------------------------------- |
| REQ01: Atualizar estoque | CT01: Reduzir estoque após venda |
| REQ02: Importar pedidos  | CT02: Validar dados importados   |

---

### 🎯 Abrangência dos Testes

**Funcionalidades**:

* Sincronização de produtos
* Atualização de estoque
* Importação e processamento de pedidos
* Geração de relatórios

**Cases de Uso**:

* **Sucesso**: produto criado, pedido sincronizado
* **Falha**: campos ausentes, status inválido
* **Carga**: envio em lote, sincronização massiva

**Priorização**:

| Cenário                         | Impacto | Frequência | Prioridade |
| ------------------------------- | ------- | ---------- | ---------- |
| Erro na importação de pedidos   | Alta    | Alta       | Alta       |
| Falha na atualização de estoque | Alta    | Alta       | Alta       |
| Problema ao exportar relatório  | Baixo   | Média      | Média      |

---

### 🛠 Execução dos Testes

**Dados**: produtos fictícios, pedidos variados, estoques simulados  
**Automação**: Postman, Selenium/Cypress, JMeter, CI/CD  
**Documentação**: TestRail, relatórios automatizados, logs, planilhas

---

### 📌 Considerações Finais

* Testes em ambiente isolado
* Validação de erros
* Garantia de resiliência

---

## Cenário 3 Erro de Anúncio – Pausado sem Estoque 

### 📌 Resumo do Problema

* Estoque zerado não muda para "Pausado (sem estoque)"
* Anúncio continua visível no Mercado Livre
* Nenhum envio de `available_quantity = 0` registrado

---

### 🔍 Análise

* API exige `available_quantity = 0` para mudar status
* Nenhum log confirma envio
* ERP não dispara atualização automaticamente

---

### 💡 Hipóteses

| # | Hipótese                               |
| - | -------------------------------------- |
| 1 | ERP não envia atualização automática   |
| 2 | Lógica interna apenas pausa            |
| 3 | Delay ou falha no serviço              |
| 4 | Configuração do cliente bloqueia envio |

---

### 🛠 Possiveis Soluções

1. Revisar lógica de envio ao zerar estoque
2. Criar testes automatizados para validar status
3. Registrar eventos de estoque zerado
4. Checar necessidade de configuração extra

---

## Cenário 4 Validação de Dados Cadastrais 

### 📌 Campos e Testes

**Campos**:

* Nome completo
* E-mail
* Telefone
* Data de nascimento
* Endereço (rua, cidade, estado, CEP)

---

### 🧪 Tipos de Teste por Campo

* **Nome completo**: formato, caracteres inválidos, tamanho
* **E-mail**: formato válido/inválido, obrigatório, tamanho
* **Telefone**: DDD, máscara, caracteres inválidos
* **Data de nascimento**: formato válido, maioridade
* **Endereço**: campos obrigatórios, caracteres inválidos, API de CEP

---

### 🔄 Testes Gerais

* Persistência e edição parcial
* Cancelar edição sem salvar
* Exportação correta
* Edições simultâneas

---

### 🎨 Testes de Interface

* Máscaras automáticas
* Mensagens de erro claras
* Botão "Gravar" desabilitado com erros

---

### 📌 Conclusão

Este cenário garante validação **técnica e de usabilidade**, respeitando:

* Regras de negócio
* Integridade de dados
* Experiência do usuário

---
