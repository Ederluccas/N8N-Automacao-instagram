# 🚀 n8n Nova Versão - Infraestrutura Produção

> Migração completa do n8n para infraestrutura robusta, escalável e produção-ready em DigitalOcean.

**Data:** 13 de Janeiro de 2026 | **Status:** ✅ Operacional | **Uptime:** 24/7

---

## 📋 Visão Geral

Este projeto documenta a criação e configuração de uma infraestrutura de produção para n8n (Workflow Automation Platform) em um droplet Ubuntu DigitalOcean com:

- ✅ HTTPS Automático (Let's Encrypt)
- ✅ Reverse Proxy com Caddy
- ✅ Monitoramento 24/7 (UptimeRobot)
- ✅ Backups Automáticos Diários
- ✅ Docker Containers Persistentes
- ✅ Domínio Customizado (n8ninstacianfa.dev)
- ✅ IP Reservado para Zero-Downtime Migration

---

## 🌐 Acesso

| Serviço | URL/Detalhes |
|---------|-------------|
| **n8n Dashboard** | https://n8ninstacianfa.dev |
| **Usuário** | `admin` |
| **Senha** | `tZN/@e2wcG8t9e?` |
| **SSH** | `ssh n8n-droplet-new` (alias configurado) |
| **IP Droplet** | 68.183.127.25 (temporário) |
| **IP Reservado** | 167.172.3.140 (será reatribuído após 7 dias) |

---

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────┐
│     n8ninstacianfa.dev (HTTPS)          │
│    DigitalOcean Cloud Firewall          │
└──────────────┬──────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│   Caddy 2.10.2 (Reverse Proxy + SSL)    │
│   • Porta 80 → 443 (redirect)           │
│   • Let's Encrypt Automático            │
│   • Renovação Automática                │
└──────────────┬──────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│    n8n 2.3.2 (Docker Container)         │
│    • Porta 5678 (interno)               │
│    • SQLite Database (persistente)      │
│    • Autenticação Básica (admin)        │
└──────────────┬──────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│   Volumes Docker Persistentes           │
│   • /home/node/.n8n (workflows)         │
│   • n8n_data (backup automático)        │
└─────────────────────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│   DigitalOcean Backups Automáticos      │
│   • Snapshot diário do droplet          │
│   • Retenção: 7 dias                    │
│   • Recuperação em minutos              │
└─────────────────────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│   UptimeRobot Monitoramento 24/7        │
│   • Monitor HTTPS (aplicação)           │
│   • Monitor Ping (servidor)             │
│   • Alertas por Email                   │
│   • Intervalo: 5 minutos                │
└─────────────────────────────────────────┘
```

---

## 💻 Especificações Técnicas

### Servidor
| Item | Valor |
|------|-------|
| **Cloud Provider** | DigitalOcean |
| **Região** | NYC1 (New York) |
| **Sistema Operacional** | Ubuntu 22.04.5 LTS |
| **CPU** | 1 vCPU |
| **RAM** | 2 GB |
| **Armazenamento** | 48 GB SSD |
| **Kernel** | 5.15.0-113-generic |

### Aplicações
| Componente | Versão | Função |
|-----------|--------|--------|
| **n8n** | 2.3.2 | Workflow Automation |
| **Caddy** | 2.10.2 | Reverse Proxy + SSL |
| **Docker** | Latest | Container Runtime |
| **Docker Compose** | Latest | Orquestração |

### Banco de Dados
| Tipo | Status | Futuro |
|------|--------|--------|
| **Atual** | SQLite (local) | Estável para testes |
| **Futuro** | Supabase PostgreSQL | Quando IPv6 estiver pronto |

---

## 📊 Custos Mensais

| Serviço | Valor | Observação |
|---------|-------|-----------|
| Droplet 1vCPU/2GB | $2.00 | Ubuntu 22.04 LTS |
| Backups Automáticos | $0.40 | +20% (snapshots diários) |
| IP Reservado | $0.00 | Gratuito quando atribuído |
| **TOTAL** | **$2.40** | Infraestrutura Completa |

**💡 Nota:** Upgrade futuro para 2vCPU/4GB custaria ~$12/mês (recomendado para produção pesada).

---

## ⏱️ Timeline de Implementação

### ✅ Fase 1: Setup Inicial (Concluído)

- [x] Criar novo droplet Ubuntu do zero (68.183.127.25)
- [x] Instalar Docker, Docker Compose, SSH Keys
- [x] Configurar n8n com SQLite persistente
- [x] Testar acesso via tunnel SSH
- [x] Liberar portas 80/443 no Cloud Firewall
- [x] Configurar Caddy com reverse proxy
- [x] Obter certificado SSL Let's Encrypt automático
- [x] Ativar backups automáticos diários
- [x] Configurar monitoramento UptimeRobot
- [x] Criar DNS A record (n8ninstacianfa.dev → 68.183.127.25)

### 🔄 Fase 2: Teste de Estabilidade (Em Andamento)

- [ ] Monitorar UptimeRobot por 7 dias (deve estar 100% UP)
- [ ] Verificar alertas de email diariamente
- [ ] Testar workflows críticos
- [ ] Validar logs do n8n (/root/n8n/docker-compose logs)
- [ ] Confirmar backups automáticos funcionando
- [ ] Manter droplet antigo (167.172.3.140) como fallback

### 📋 Fase 3: Migração Permanente (Após 7 Dias)

- [ ] Habilitar IPv6 no droplet (já ativado)
- [ ] Conectar Supabase PostgreSQL (quando IPv6 funcionar)
- [ ] Migrar dados SQLite → PostgreSQL
- [ ] Reatribuir IP Reservado 167.172.3.140 para novo droplet
- [ ] Validar DNS pointing correto
- [ ] Destruir droplet antigo (libera ~$2/mês)

---

## 🔧 Comandos Úteis

### Acessar o Servidor
```bash
# Via SSH (alias configurado)
ssh n8n-droplet-new

# Ou direto
ssh -i ~/.ssh/digitalocean_n8n root@68.183.127.25
```

### Gerenciar Containers
```bash
# Entrar na pasta
cd /root/n8n

# Ver status
docker-compose ps

# Ver logs em tempo real
docker-compose logs -f n8n
docker-compose logs -f caddy

# Reiniciar
docker-compose restart

# Parar
docker-compose down

# Iniciar
docker-compose up -d
```

### Verificar Saúde
```bash
# Status do sistema
uptime
free -h
df -h

# Verificar conectividade HTTPS
curl -I https://n8ninstacianfa.dev

# Verificar certificado SSL
echo | openssl s_client -servername n8ninstacianfa.dev -connect n8ninstacianfa.dev:443 2>/dev/null | openssl x509 -noout -dates
```

### Backup Manual
```bash
# Fazer backup do volume n8n_data
docker run --rm -v n8n_data:/data -v $(pwd):/backup alpine tar czf /backup/n8n_backup_$(date +%Y%m%d_%H%M%S).tar.gz /data
```

---

## 🔐 Segurança

- ✅ **HTTPS Obrigatório** com redirect HTTP → HTTPS
- ✅ **SSL/TLS** renovado automaticamente (Let's Encrypt)
- ✅ **Autenticação Básica** no n8n (usuário + senha)
- ✅ **Firewall** liberando apenas portas 80, 443, 5678
- ✅ **SSH Keys** Ed25519 (sem acesso por senha)
- ✅ **Backups Automáticos** para recuperação de emergência

---

## 📈 Monitoramento

### UptimeRobot
- **Monitor 1:** HTTPS (aplicação) - verifica se n8n responde
- **Monitor 2:** Ping (servidor) - verifica se droplet está online
- **Intervalo:** 5 minutos
- **Alertas:** Email quando algum monitor ficar DOWN
- **Histórico:** 24h/7d/30d disponível no dashboard

### Logs do Servidor
```bash
# Verificar logs do n8n
docker-compose logs --tail 100 n8n

# Verificar logs do Caddy
docker-compose logs --tail 100 caddy

# Logs do sistema
journalctl -u docker -n 50
```

---

## 🚀 Próximos Passos

### Curto Prazo (Esta Semana)
1. ✅ Manter monitoramento 24/7 ativo
2. ✅ Testar workflows críticos
3. ✅ Confirmar alertas funcionando
4. ✅ Validar backup automático

### Médio Prazo (Próximas 2 Semanas)
1. 🔄 Migrar para Supabase PostgreSQL (IPv6)
2. 🔄 Reatribuir IP Reservado (167.172.3.140)
3. 🔄 Destruir droplet antigo

### Longo Prazo (1-3 Meses)
1. ⬜ Implementar webhook do Instagram
2. ⬜ Upgrade para 2vCPU/4GB (se necessário)
3. ⬜ Configurar CI/CD com GitHub Actions
4. ⬜ Escalar para múltiplos workers (se necessário)

---

## 📚 Documentação Interativa

Veja **README.html** para uma visualização interativa e moderna desta documentação!

```bash
# Abrir no navegador
open README.html  # macOS
xdg-open README.html  # Linux
start README.html  # Windows
```

---

## 🔗 Links Importantes

### Acesso
- **n8n Dashboard:** https://n8ninstacianfa.dev
- **DigitalOcean Console:** https://cloud.digitalocean.com/droplets
- **UptimeRobot Dashboard:** https://uptimerobot.com

### Ferramentas
- **DigitalOcean CLI:** `doctl` (para gerenciamento via terminal)
- **SSH Config:** `~/.ssh/config` (configuração de atalhos)

---

## 📝 Notas Importantes

### Backup de Segurança
Sempre faça backup antes de grandes mudanças:
```bash
docker run --rm -v n8n_data:/data -v $(pwd):/backup alpine tar czf /backup/n8n_backup.tar.gz /data
```

### Escalabilidade Futura
Para crescimento, considere:
- Upgrade de RAM (2GB → 4GB)
- Database externa (PostgreSQL em Supabase)
- Load balancer se múltiplos n8n
- Kubernetes (se muitos containers)

### Troubleshooting Comum
```bash
# n8n lento ou travado?
docker-compose restart n8n

# Certificado SSL expirado?
docker-compose logs caddy | grep "certificate"

# Sem conexão ao servidor?
ssh -vvv n8n-droplet-new  # verbose output

# Volume de dados perdido?
# Restaurar do backup automático via DigitalOcean dashboard
```

---

## 👨‍💻 Stack Tecnológico

- **Containerização:** Docker + Docker Compose
- **Reverse Proxy:** Caddy 2
- **SSL/TLS:** Let's Encrypt (Automático)
- **Monitoramento:** UptimeRobot + Email
- **Backup:** DigitalOcean Snapshots
- **DNS:** DigitalOcean DNS
- **IaC:** docker-compose.yml (git versioned)

---

## 📄 Licença

Este projeto documenta uma infraestrutura de produção para n8n.

---

**Última Atualização:** 13 de Janeiro de 2026  
**Status:** ✅ Operacional  
**Uptime:** 24/7 com Monitoramento  
**Suporte:** Backups automáticos + Redundância planejada