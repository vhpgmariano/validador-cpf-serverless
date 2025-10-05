# validador-cpf-serverless

---

# 📄 README.md: Microsserviço Serverless de Validação de CPF (Azure Functions)

## 🎯 Objetivo do Projeto

Desenvolver um microsserviço eficiente, escalável e econômico para a validação de CPFs (Cadastro de Pessoa Física). A solução será construída com base em uma arquitetura **Serverless** no Azure, garantindo **alta disponibilidade**, **baixo custo operacional** (pagamento por execução) e **facilidade de manutenção**.

## 🚀 Arquitetura Serverless Recomendada (Azure)

Esta arquitetura utiliza os serviços Serverless do Azure para lidar com picos de demanda e manter os custos baixos.

| Componente | Serviço do Azure | Função no Microsserviço |
| :--- | :--- | :--- |
| **Motor de Execução** | **Azure Functions** (Plano de Consumo) | Recebe a requisição HTTP e executa a lógica de validação. Escala automaticamente para zero em ociosidade. |
| **Segurança & Gateway** | **Azure API Management (APIM)** | Atua como *gateway* de API. Aplica limites de taxa (*Rate Limiting*), autenticação (via Tokens JWT) e *caching*. |
| **Lógica de Negócios** | **Azure Functions** (Código) | Implementa a regra de validação do CPF (cálculo e verificação dos dígitos). |
| **Gerenciamento de Segredos** | **Azure Key Vault** | Armazena chaves de API ou segredos de forma segura, acessível via **Identidades Gerenciadas**. |

## ✨ Princípios-Chave (Eficiência e Economia)

O pilar deste projeto é o uso do **Azure Functions no Plano de Consumo**:

1.  **Custo por Uso:** Pagamento estrito pelo tempo de execução (em milissegundos) e pela memória consumida. Custo é zero quando o serviço não está sendo chamado.
2.  **Escalabilidade Serverless:** O Azure gerencia a infraestrutura, escalando o número de instâncias instantaneamente para lidar com picos de tráfego e desativando-as em momentos de baixa demanda.
3.  **Segurança Sem Credenciais:** Uso de **Identidades Gerenciadas** para que o Azure Function acesse o Key Vault, eliminando a necessidade de armazenar senhas ou chaves em código.

---

## 🛠️ Roteiro de Implementação

### Passo 1: Configuração do Azure Function

1.  Crie um novo projeto de **Azure Function** com o *template* **HTTP Trigger**.
2.  Configure o *endpoint* para aceitar requisições **POST** (ex: `/api/validar-cpf`).
3.  Escolha uma linguagem eficiente para a lógica (*e.g.*, C\#/.NET ou Python) para minimizar a latência.
4.  Implante o Function no **Plano de Consumo** do Azure.

### Passo 2: Lógica de Validação

1.  Implemente a função que receberá o CPF do corpo (body) da requisição HTTP.
2.  Adicione a lógica de validação do CPF (cálculo dos dois dígitos verificadores).
3.  Retorne uma resposta HTTP 200 (OK) com um objeto JSON simples, como `{"status": "VALIDO"}` ou `{"status": "INVALIDO"}`.

### Passo 3: Segurança com API Management (APIM)

1.  Crie uma instância do **Azure API Management**.
2.  Importe a Azure Function como uma nova API no APIM.
3.  Aplique as seguintes **políticas** no APIM:
    * **Limitação de Taxa:** Proteja o *backend* aplicando limites de chamadas por cliente.
    * **Autenticação:** Configure a política para validar tokens de segurança (JWTs) do Azure Entra ID antes de encaminhar a requisição.

### Passo 4: Gerenciamento de Segredos

1.  Crie um **Azure Key Vault** para armazenar quaisquer segredos (como *strings* de conexão se o microsserviço usasse um banco de dados).
2.  Habilite uma **Identidade Gerenciada** no Azure Function.
3.  Use o **Azure RBAC** para conceder à Identidade Gerenciada a permissão de **leitura** (Get/List) de segredos no Key Vault. O código acessará o segredo sem credenciais embutidas.

---

## 📝 Como Executar Localmente

*(Esta seção deve ser preenchida com as instruções específicas da linguagem escolhida, por exemplo:)*

1.  Clone o repositório.
2.  Instale o **Azure Functions Core Tools**.
3.  Rode `func start` no terminal do projeto.
4.  Teste o *endpoint* com um cliente HTTP (como o Postman) em `http://localhost:7071/api/validar-cpf`.
