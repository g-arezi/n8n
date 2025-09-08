# N8N Automation Platform - Tutorial Completo

## 📋 Índice
- [O que é o n8n](#o-que-é-o-n8n)
- [Instalação](#instalação)
- [Primeiros Passos](#primeiros-passos)
- [Conceitos Básicos](#conceitos-básicos)
- [Criando seu Primeiro Workflow](#criando-seu-primeiro-workflow)
- [Configuração do Telegram Bot](#configuração-do-telegram-bot)
- [Exemplos de Workflows](#exemplos-de-workflows)
- [Dicas e Melhores Práticas](#dicas-e-melhores-práticas)
- [Troubleshooting](#troubleshooting)

## 🤖 O que é o n8n

O n8n é uma plataforma de automação de código aberto que permite conectar diferentes serviços e automatizar tarefas sem necessidade de programação complexa. Com interface visual intuitiva, você pode criar workflows que integram APIs, bancos de dados, webhooks e muito mais.

## 🚀 Instalação

### Com Docker (Recomendado)

1. **Instalar Docker Desktop**
   - Baixe e instale o Docker Desktop para Windows
   - Certifique-se de que está executando

2. **Executar o n8n**
   ```powershell
   docker run -it --rm -p 5678:5678 -v d:\n8n:/home/node/.n8n n8nio/n8n
   ```

3. **Acessar a Plataforma**
   - Abra seu navegador e acesse: http://localhost:5678

### Estrutura de Pastas
```
d:\n8n/
├── config              # Configurações da aplicação
├── database.sqlite     # Banco de dados local
├── n8nEventLog.log    # Logs de eventos
├── binaryData/        # Arquivos binários
├── nodes/             # Nós customizados
├── ssh/               # Chaves SSH
└── git/               # Repositórios Git
```

## 🎯 Primeiros Passos

### 1. Configuração Inicial
- Acesse http://localhost:5678
- Crie sua conta de usuário
- Configure suas credenciais básicas

### 2. Interface Principal
- **Canvas**: Área principal onde você constrói workflows
- **Painel de Nós**: Lista de serviços disponíveis (lado esquerdo)
- **Painel de Configuração**: Configurações do nó selecionado (lado direito)
- **Barra de Ferramentas**: Executar, salvar, configurações

## 📚 Conceitos Básicos

### Workflows
Um workflow é uma sequência de nós conectados que automatizam uma tarefa específica.

### Nós (Nodes)
- **Trigger Nodes**: Iniciam o workflow (webhook, timer, manual)
- **Regular Nodes**: Executam ações (APIs, banco de dados, transformações)
- **Output Nodes**: Finalizam o workflow (envio de dados, notificações)

### Conexões
- Conectam nós para definir o fluxo de dados
- Dados passam de um nó para o próximo
- Suportam condicionais e ramificações

### Execuções
- Cada vez que um workflow roda, cria uma execução
- Histórico de execuções fica salvo
- Permite debug e monitoramento

## 🔧 Criando seu Primeiro Workflow

### Exemplo: Monitor de Site + Notificação Telegram

1. **Adicionar Trigger**
   - Clique em "Add first step"
   - Selecione "Schedule Trigger"
   - Configure para executar a cada 5 minutos

2. **Adicionar HTTP Request**
   - Conecte um nó "HTTP Request"
   - Configure URL do site para monitorar
   - Método: GET

3. **Adicionar Condição**
   - Adicione nó "IF"
   - Configure condição para status code ≠ 200

4. **Adicionar Telegram**
   - No ramo "true" da condição
   - Adicione nó "Telegram"
   - Configure bot token e chat ID

## 📱 Configuração do Telegram Bot

### Criando um Bot

1. **Contatar o BotFather**
   - Abra o Telegram
   - Procure por @BotFather
   - Envie `/newbot`
   - Siga as instruções

2. **Obter Token**
   - Salve o token fornecido
   - Exemplo: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`

### Configurando no n8n

1. **Adicionar Credencial**
   - Vá em Settings > Credentials
   - Clique em "Create New"
   - Selecione "Telegram"
   - Cole o token do bot

2. **Obter Chat ID**
   ```javascript
   // No nó Function, use este código:
   return [
     {
       json: {
         chat_id: $node["Webhook"].json["message"]["chat"]["id"],
         text: "Olá! Seu chat ID é: " + $node["Webhook"].json["message"]["chat"]["id"]
       }
     }
   ];
   ```

### Erros Comuns do Telegram

❌ **"Bad Request: chat not found"**
- **Causa**: Chat ID incorreto ou bot não iniciado
- **Solução**: Use ID numérico, não @username

❌ **"Unauthorized"**
- **Causa**: Token inválido
- **Solução**: Verifique o token do BotFather

## 💡 Exemplos de Workflows

### 1. Backup Automático de Banco de Dados
```
Schedule Trigger → Execute Command → Compress File → Send to Cloud Storage → Telegram Notification
```

### 2. Monitor de Preços
```
Schedule Trigger → HTTP Request (API preços) → IF (preço mudou) → Save to Database → Email Alert
```

### 3. Processamento de Formulários
```
Webhook → Validate Data → Save to Database → Send Confirmation Email → Slack Notification
```

### 4. Integração GitHub → Discord
```
GitHub Trigger → Format Message → Discord Webhook → Log to File
```

## 🎯 Dicas e Melhores Práticas

### Performance
- Use `Batch Size` para processar grandes volumes
- Configure `Timeout` adequadamente
- Utilize `Function` nodes para transformações complexas

### Segurança
- Sempre use credenciais para APIs
- Não exponha tokens em logs
- Configure webhooks com autenticação

### Debug
- Use `Edit Fields` para inspecionar dados
- Ative logs detalhados quando necessário
- Teste workflows com dados reais

### Organização
- Use nomes descritivos para workflows
- Adicione comentários nos nós
- Organize workflows em pastas

## 🔧 Troubleshooting

### Problemas Comuns

**Workflow não executa**
1. Verifique se está ativo
2. Confirme configuração do trigger
3. Verifique logs de erro

**Erro de credenciais**
1. Reconfigure a credencial
2. Teste a conexão
3. Verifique permissões da API

**Dados não passam entre nós**
1. Verifique estrutura JSON
2. Use `Expression` para mapear campos
3. Confirme se conexões estão corretas

**Performance lenta**
1. Otimize queries de banco
2. Reduza batch size
3. Adicione cache quando possível

### Logs e Monitoramento
- Logs ficam em: `n8nEventLog.log`
- Use o painel de execuções para debug
- Configure alertas para falhas críticas

## 📞 Recursos Adicionais

- **Documentação Oficial**: https://docs.n8n.io
- **Community**: https://community.n8n.io
- **Templates**: https://n8n.io/workflows
- **GitHub**: https://github.com/n8n-io/n8n

## 🏷️ Versão
Este tutorial foi criado para n8n versão mais recente em Docker.

---

**Criado em**: Setembro 2025  
**Última atualização**: Setembro 2025