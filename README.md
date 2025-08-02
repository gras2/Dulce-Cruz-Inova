# MVP - Integração e Automação para Clínica Odontológica com Notion, Google Sheets e Google Apps Script

Este repositório descreve um **MVP (Minimum Viable Product)** para a integração e automação de dados em uma clínica odontológica, utilizando **Notion**, **Google Sheets**, e **Google Apps Script**.

### **Objetivo**

Criar um sistema de gestão de dados de clientes, controle financeiro, indicações e compras, com automação para análise de custos, previsibilidade de demanda e visualização de KPIs.

---

## **Tecnologias Utilizadas**

- **Notion**: Armazenamento de dados para **CRM**, **indicações**, **controle de compras** e **financeiro**.
- **Notion API**: Para integração e automação de fluxos entre o **Google Sheets** e o **Notion**.
- **Google Sheets**: Para análise e processamento de dados.
- **Google Apps Script**: Para automação de tarefas e integração entre **Google Sheets** e **Notion**.
- **Google Data Studio** (ou Power BI): Para criação de dashboards interativos e visualização de dados.

---

## **Passos para Implementação**

### **1. Estruturação do Notion para Armazenamento de Dados**

#### **1.1: Criar Bancos de Dados no Notion**
1. Crie as páginas de banco de dados no **Notion** para armazenar os seguintes dados:
   - **CRM**: Informações sobre os clientes.
   - **Indicações**: Para monitorar os pacientes indicados.
   - **Controle de Compras**: Para controlar os materiais e serviços comprados.
   - **Controle Financeiro**: Para registrar pagamentos e receitas.

#### **1.2: Estruturar as Colunas nos Bancos de Dados**
- **CRM**:
  - Nome do Cliente
  - Procedimento
  - Valor do Procedimento
  - Telefone
  - Tipo de Plano
  - Canal de origem
  - Forma de pagamento
  - Contato de emergência
  - Histórico de interações
  - Data do agendamento
  - Status das solicitações do plano
  - Status do Agendamento
  - Próximos passos
- **Indicações**:
  - Nome de quem indicou
  - Nome do paciente
- **Modelo de Compras**:
  - Material
  - Custo do Material
  - Fornecedor
  - Quantidade
  - Custo Unitário
  - Data de Compra
  - Material por Procedimento
- **Controle Financeiro**:
  - Valor Pago
  - Data de Pagamento
  - Cliente
  - Status do Pagamento (Pendente, Pago)
  - Custo de Materiais

---

### **2. Integração com Notion API**

#### **2.1: Configurar a Notion API**
1. Acesse [Notion Developers](https://www.notion.so/my-integrations) e crie uma nova **integração** para obter a **chave de API**.
2. Obtenha o **ID do banco de dados** para que você possa interagir com os dados.

#### **2.2: Criar Scripts no Google Apps Script**
1. Acesse **Google Sheets** e abra o **Editor de Apps Script** (`Extensões > Apps Script`).
2. Crie um script para enviar dados do **Google Sheets** para o **Notion** via **Notion API**.

**Exemplo de código em Python para integração com Notion**:

```python
import requests

# URL da API do Notion
notion_url = "https://api.notion.com/v1/pages"

# Headers com token de autenticação e versão do Notion
headers = {
    "Authorization": "Bearer YOUR_NOTION_API_KEY",
    "Content-Type": "application/json",
    "Notion-Version": "2021-05-13",
}

# Dados a serem enviados para o Notion
data = {
    "parent": {"database_id": "YOUR_DATABASE_ID"},
    "properties": {
        "Nome do Cliente": {"title": [{"text": {"content": "Cliente X"}}]},
        "Procedimento": {"rich_text": [{"text": {"content": "Clareamento"}}]},
        "Valor do Procedimento": {"number": 200},
        "Telefone": {"rich_text": [{"text": {"content": "9999-9999"}}]},
    }
}

response = requests.post(notion_url, headers=headers, json=data)
print(response.json())
```

#### **2.3: Sincronizar Dados entre Google Sheets e Notion**
Use o Google Apps Script para enviar dados automaticamente do Google Sheets para o Notion. A cada novo registro na planilha, o script envia as informações para a tabela correspondente no Notion.

```javascript
function sincronizarDadosGoogleSheetsComNotion() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('CRM');
  var lastRow = sheet.getLastRow();
  var nomeCliente = sheet.getRange(lastRow, 1).getValue(); // Nome do Cliente
  var procedimento = sheet.getRange(lastRow, 2).getValue(); // Procedimento
  var valor = sheet.getRange(lastRow, 3).getValue(); // Valor do Procedimento

  // Definindo a URL da API do Notion
  var notionUrl = 'https://api.notion.com/v1/pages';
  var notionToken = 'YOUR_NOTION_API_KEY';  // Substitua pela sua chave de API
  var databaseId = 'YOUR_DATABASE_ID';  // Substitua pelo ID do banco de dados

  // Payload para enviar para o Notion
  var payload = JSON.stringify({
    parent: { database_id: databaseId },
    properties: {
      "Nome do Cliente": { "title": [{ "text": { "content": nomeCliente }}] },
      "Procedimento": { "rich_text": [{ "text": { "content": procedimento }}] },
      "Valor do Procedimento": { "number": valor }
    }
  });

  // Opções para a requisição POST
  var options = {
    "method": "POST",
    "headers": {
      "Authorization": "Bearer " + notionToken,
      "Content-Type": "application/json",
      "Notion-Version": "2021-05-13"
    },
    "payload": payload
  };

  // Enviando a requisição para o Notion
  var response = UrlFetchApp.fetch(notionUrl, options);
  Logger.log(response.getContentText());
}
```

## 3. Automação de Cálculos e Análises no Notion

### 3.1: Realizar Cálculos e Análises no Notion

Embora o **Notion** não tenha suporte completo para cálculos complexos, você pode configurar **propriedades de fórmula** no Notion para realizar alguns cálculos básicos.

### Exemplos de Fórmulas:

- **Valor Gasto com Material por Procedimento**: Calcule o custo de cada material com base no **custo unitário** e **quantidade de materiais** usados em cada procedimento.

  **Fórmula exemplo no Notion**:
  ```markdown
  Custo Total = Custo Unitário * Quantidade
Previsibilidade de Demanda: Use fórmulas para calcular o número de vezes que um procedimento foi realizado e prever quais materiais serão mais utilizados.

Procedimento Mais Realizado: Use a contagem de registros para ver qual procedimento foi mais realizado no mês.

### **4. Criação de Dashboards Interativos com Google Data Studio**
**4.1: Conectar o Notion com Google Data Studio**
Para criar dashboards no Google Data Studio a partir dos dados do Notion, use ferramentas de integração como Apipheny ou Coupler.io, que conectam o Google Sheets ao Notion.

Passos:
Use Apipheny ou Coupler.io para sincronizar os dados do Notion com o Google Sheets.

Crie relatórios no Google Data Studio, conectando as planilhas do Google Sheets.

Personalize o relatório para visualizar KPIs como:

Taxa de Conversão de Indicações

Custo por Procedimento

Procedimento Mais Realizado

### **5. Testes e Ajustes Finais**
**5.1: Testar Integrações e Automação**
 - Teste o envio de dados do Google Sheets para o Notion.

 - Verifique se os dados estão sendo corretamente inseridos no Notion.

 - Teste as fórmulas no Notion para garantir que os cálculos de custos de material e procedimentos mais realizados estejam corretos.

**5.2: Refinamento do Sistema**
Ajuste qualquer erro encontrado durante os testes.
Refine os dashboards no Google Data Studio para que as visualizações sejam claras e úteis.
### **Tempo Estimado para Implementação:**
 - Estruturação no Notion: 1 dia
 - Integração com Notion API: 2-3 dias
 - Automação e Cálculos no Notion: 2 dias
 - Criação de Dashboards no Google Data Studio: 1-2 dias
 - Testes e Ajustes Finais: 1-2 dias
 - Tempo Total Estimado: 6-10 dias
