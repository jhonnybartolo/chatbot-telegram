# 🌤️ Bot de Clima no Telegram com N8N

Chatbot no Telegram que informa a temperatura atual de qualquer cidade do Brasil. O usuário envia o nome da cidade e o bot responde com a temperatura em tempo real consultando a API da OpenWeather.

---

## 📌 Como funciona

O usuário envia uma mensagem no formato `Cidade,UF,BR` para o bot no Telegram. O workflow no n8n recebe a mensagem, consulta a API da OpenWeather e responde com a temperatura atual da cidade.

| Situação | Exemplo |
|---|---|
| Usuário envia | `São Paulo,SP,BR` |
| Bot responde | `🌤️ A temperatura em São Paulo é de 24°C.` |
| Usuário envia cidade inválida | `Blablabla,XX,BR` |
| Bot responde | `❌ Cidade não encontrada. Use o formato Cidade,UF,BR (ex.: São Paulo,SP,BR).` |

---

## ✅ Pré-requisitos

- N8N instalado e rodando
- Conta no [Telegram](https://telegram.org/)
- Conta na [OpenWeather](https://openweathermap.org/) (plano gratuito é suficiente)

---

## 🔑 Credenciais necessárias

### 1. TELEGRAM_BOT_TOKEN

Token de acesso do bot no Telegram utilizado neste projeto:

```
TELEGRAM_BOT_TOKEN="8283200162:AAHoNqkfHB-cIqBc8MaWKP0qoUAZAnyivRU"
```

**Como inserir no N8N:**

1. No nó **Telegram Trigger**, clique em **"Credential for Telegram API"**
2. Clique em **"Create new credential"**
3. No campo **"Access Token"** insira o valor de `TELEGRAM_BOT_TOKEN`
4. Clique em **Save**
5. Repita o mesmo processo nos nós **Telegram Send Message**

---

### 2. OPENWEATHER_API_KEY

Chave de acesso da API OpenWeather utilizada neste projeto:

```
OPENWEATHER_API_KEY="cacc8f0179d599307293bd12477c2302"
```

**Como inserir no N8N:**

1. No nó **HTTP Request**, em **Authentication** selecione **"Generic Credential Type"**
2. Em **Generic Auth Type** selecione **"Query Auth"**
3. Clique em **"Create new credential"**
4. Preencha os campos:
   - **Name:** `appid`
   - **Value:** insira o valor de `OPENWEATHER_API_KEY`
5. Clique em **Save**

---

## 📥 Como importar o workflow no N8N

1. Faça o download do arquivo `workflow-chatbot-telegram.json` deste repositório
2. Abra o N8N
3. Clique em **"Add workflow"** → **"Import from file"**
4. Selecione o arquivo `workflow-chatbot-telegram.json`
5. O workflow será importado com todos os nós configurados
6. Configure as credenciais conforme instruções acima
7. Clique em **"Publish"** no canto superior direito para ativar o bot

---

## 🤖 Como executar o chatbot

### Acessar o bot

O bot está disponível no Telegram pelo link:
*Também é posivel procurar o bot no chat de telegram, colocando o nome "projetomodulo2_bot"

👉 [https://t.me/projetomodulo2_bot](https://t.me/projetomodulo2_bot)

### Enviar uma cidade de teste

Após ativar o workflow no N8N, abra o bot no Telegram e envie uma mensagem no formato:

```
Cidade,UF,BR
```

### Exemplos de teste e retorno esperado

**Teste 1 — Cidade válida:**

```
São Paulo,SP,BR
```
> 🌤️ A temperatura em São Paulo é de 24°C.

---

**Teste 2 — Outra cidade válida:**

```
Curitiba,PR,BR
```
> 🌤️ A temperatura em Curitiba é de 18°C.

---

**Teste 3 — Cidade inválida (teste de erro):**

```
Blablabla,XX,BR
```
> ❌ Cidade não encontrada. Use o formato Cidade,UF,BR (ex.: São Paulo,SP,BR).

---

## 🔀 Estrutura do workflow

```
Telegram Trigger
      ↓
Edit Fields (formata a cidade → variável queue)
      ↓
HTTP Request (consulta OpenWeather)
      ↓
IF node (verifica se a resposta é válida)
   ↙                    ↘
true                   false
  ↓                      ↓
Code node            Telegram Send
(extrai e            (mensagem de erro)
formata temp.)
  ↓
Telegram Send
(mensagem de sucesso)
```

---

## 🗂️ Nós do workflow

| Nó | Tipo | Função |
|---|---|---|
| Telegram Trigger | Trigger | Recebe mensagens do usuário |
| Edit Fields | Set | Captura e normaliza o texto na variável `queue` |
| HTTP Request | HTTP | Consulta a API OpenWeather |
| IF | Condicional | Valida se a resposta contém temperatura |
| Code in JavaScript | Code | Extrai e arredonda a temperatura |
| Send a text message | Telegram | Envia a temperatura ao usuário |
| Send a text message1 | Telegram | Envia mensagem de erro ao usuário |

---
