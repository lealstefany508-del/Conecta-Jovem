```markdown
<h1 align="center">🎓 Conecta Jovem</h1>

<p align="center">
  Plataforma web responsiva e instalável (PWA) para conectar estudantes a oportunidades de estágio e jovem aprendiz,<br>
  com área do estudante, painel do recrutador e administração operacional.
</p>

<p align="center">
  <img alt="Node.js" src="[https://img.shields.io/badge/Node.js-24-339933?logo=nodedotjs&logoColor=white](https://img.shields.io/badge/Node.js-24-339933?logo=nodedotjs&logoColor=white)">
  <img alt="React" src="[https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)">
  <img alt="Express" src="[https://img.shields.io/badge/Express-5-000000?logo=express&logoColor=white](https://img.shields.io/badge/Express-5-000000?logo=express&logoColor=white)">
  <img alt="PostgreSQL" src="[https://img.shields.io/badge/PostgreSQL-Neon-4169E1?logo=postgresql&logoColor=white](https://img.shields.io/badge/PostgreSQL-Neon-4169E1?logo=postgresql&logoColor=white)">
  <img alt="Vercel" src="[https://img.shields.io/badge/Deploy-Vercel-000000?logo=vercel&logoColor=white](https://img.shields.io/badge/Deploy-Vercel-000000?logo=vercel&logoColor=white)">
</p>

<p align="center">
  <sub>Aplicação publicada em: <a href="[https://conecta-jovem-two.vercel.app](https://conecta-jovem-two.vercel.app)">conecta-jovem-two.vercel.app</a></sub>
</p>

---

## 📌 Sobre o projeto

O **Conecta Jovem** resolve a dificuldade de estudantes em encontrar vagas reais de estágio e jovem aprendiz compatíveis com seu curso, turno e cidade, ao mesmo tempo em que oferece às empresas um fluxo organizado de triagem e publicação.

A aplicação não trabalha com vagas fictícias ou geradores de dados: cada oportunidade interna é criada por um recrutador autenticado de empresa aprovada, e o catálogo externo exibe anúncios reais sincronizados via API da Adzuna com crédito e link de origem.

O sistema é dividido em duas camadas principais:

| Parte | Tecnologia | Pasta |
|---|---|---|
| **Interface (Frontend / PWA)** | React 19 + Vite + CSS responsivo | [`src/`](src) e [`public/`](public) |
| **API e Regras (Backend)** | Node.js 24 + Express 5 + PostgreSQL (Neon) / SQLite | [`server/`](server) e [`api/`](api) |

## ✨ Funcionalidades

### 👤 Estudante
- Cadastro, login e recuperação de acesso por e-mail com token de uso único
- Política etária integrada: acesso a partir de **16 anos**, com fluxo de autorização por e-mail do responsável para menores de 18 anos
- Perfil profissional estruturado: formações, habilidades, projetos, cursos complementares, idiomas, disponibilidade, modalidade e foto
- Busca sem distinção de acentos e filtros por cidade, curso, modalidade, tipo (Estágio/Aprendiz), bolsa mínima e carga horária
- Cálculo de compatibilidade explicado por critérios (**Curso/área 30%**, **Habilidades 30%**, **Disponibilidade 20%**, **Modalidade/local 20%**)
- Favoritos persistentes (vagas internas e externas) e candidatura única por vaga
- Acompanhamento de etapas em tempo real, histórico de movimentações e cancelamento pelo estudante
- Alertas diários opcionais por e-mail com confirmação dupla, controle de visibilidade do perfil, exportação de dados em JSON e exclusão de conta

### 💼 Empresa e Recrutador
- Cadastro empresarial e solicitação de aprovação administrativa (com envio de site HTTPS e comprovação de vínculo)
- Gestão de equipe pelo responsável da empresa (`owner`): adicionar, suspender ou revogar recrutadores
- Publicação, edição, pausa e encerramento de vagas internas com critérios de curso, habilidades, turno e prazo
- Triagem de candidatos com transições controladas de etapa (`Enviada → Em análise → Pré-selecionada → Entrevista → Aprovada / Não selecionada`) e registro obrigatório de motivo

### 🛠️ Administrador
- Painel operacional em `/admin` com auditoria completa (autor, data e motivo obrigatório em todas as ações)
- Aprovação, rejeição ou suspensão de empresas antes da abertura de vagas
- Moderação de contas, suspensão/liberação de vagas e análise de denúncias
- Correção excepcional de resultados finais de candidaturas com notificação ao estudante
- Monitoramento de backups diários criptografados, eventos técnicos sanitizados e respostas a solicitações de privacidade

## 🧠 Regras de negócio implementadas

| Regra | Como funciona |
|---|---|
| **RN001** | Somente estudantes autenticados, maiores de 16 anos (com autorização do responsável se menores de 18) e com perfil visível se candidatam |
| **RN002** | Cada estudante pode enviar apenas uma candidatura por vaga interna, bloqueada após o prazo ou se a vaga estiver pausada/encerrada |
| **RN003** | Apenas empresas aprovadas pela administração podem publicar ou abrir vagas internas |
| **RN004** | Recrutadores enxergam e gerenciam somente vagas e candidatos da própria empresa; gestão da equipe é exclusiva do responsável (`owner`) |
| **RN005** | As etapas de seleção seguem transições válidas e exigem registro no histórico, gerando notificação automática ao estudante |
| **RN006** | Anúncios externos (Adzuna) preservam título, empresa, localização, crédito e link original, sem inventar salário, compatibilidade ou processo interno |
| **RN007** | Perfis configurados como `Privado` ocultam os dados do estudante e bloqueiam novas candidaturas até a alteração da visibilidade |

Além disso: senhas com hash `scrypt` e *salt* aleatório, sessões em cookie `HttpOnly` (`SameSite=Strict`), proteção contra CSRF por validação de `Origin` e controle de taxa (*rate limit*) na autenticação.

## 🧰 Tecnologias

**Frontend e PWA**
- React 19, Vite 7 e ícones Lucide React
- Web App Manifest, Service Worker (`sw.js`) e página offline sem cache de dados sensíveis
- Responsividade validada em `360px`, `768px` e `1280px` e auditoria automatizada de acessibilidade com `axe-core`

**Backend, Banco e Operação**
- Node.js 24, Express 5 e `@vercel/functions`
- PostgreSQL (Neon) em produção com migrações SQL versionadas e SQLite para testes/modo local isolado
- Nodemailer (SMTP TLS) / Resend para recuperação de senha, alertas e notificações operacionais
- Backups consistentes compactados e criptografados com **AES-256-GCM** e retenção automática via IMAP

## 🗂️ Estrutura

```
Conecta-Jovem/
├── api/
│   └── index.js                      entrada serverless na Vercel
├── docs/
│   ├── DER.md                        modelo entidade-relacionamento
│   ├── DICIONARIO_DADOS.md           especificação das tabelas e colunas
│   ├── DECISOES.md                   escopo e decisões arquiteturais
│   ├── OPERACOES.md                  rotinas de backup, restauração e monitor
│   ├── PILOTO.md                     protocolo de avaliação com usuários
│   └── PUBLICACAO.md                 configuração de deploy e SMTP
├── public/
│   ├── icons/                        ícones do PWA (192x192 e 512x512)
│   ├── manifest.webmanifest          manifesto de instalação
│   ├── offline.html                  tela offline do PWA
│   ├── privacy.html                  política de privacidade pública
│   └── sw.js                         service worker
├── scripts/
│   ├── monitor.mjs                   monitoramento contínuo de saúde HTTPS
│   ├── restore-postgres.mjs          restauração validada em branch isolada
│   ├── retain-backups.mjs            retenção de backups locais e remotos
│   ├── verify-load.mjs               teste de carga (100 sessões)
│   └── verify-postgres.mjs           suíte de testes em PostgreSQL isolado
├── server/
│   ├── migrations/                   scripts SQL versionados (001 a 003)
│   ├── index.js                      rotas da API, sessões e autorização
│   ├── product.js                    alertas, aprovação, exportação e telemetria
│   ├── rules.js                      cálculo de compatibilidade e transições
│   ├── age-policy.js                 validação etária e consentimento do responsável
│   ├── adzuna.js                     integração e cache de vagas externas
│   ├── normalize.js                  persistência normalizada do perfil e vagas
│   ├── operations.js                 backup diário cifrado e estado operacional
│   └── db.js / store.js              camada de acesso PostgreSQL e SQLite
├── src/
│   ├── main.jsx                      interface principal (estudante e empresa)
│   ├── admin.jsx                     painel administrativo
│   ├── product.jsx                   alertas, privacidade e aprovação empresarial
│   ├── profile-editor.jsx            editor estruturado do perfil
│   ├── age-policy.jsx                fluxo de autorização de responsável
│   ├── search.js                     busca normalizada e filtros
│   └── style.css                     estilos responsivos
└── tests/                            suítes automatizadas (API, navegador e axe)
```

## 🚀 Como executar

### Pré-requisitos
- **Node.js 24** ou superior
- Opcional: banco **PostgreSQL (Neon)** para persistência remota (sem `DATABASE_URL`, o sistema roda localmente em SQLite)

### 1. Instalar e iniciar localmente

```bash
npm install
npm run build
npm run dev
```

Abra **`http://localhost:3000`**. Para desenvolvimento com recarga automática, utilize `npm run develop`.

> A aplicação inicia com **zero vagas, zero empresas e zero estudantes fictícios**. No primeiro arranque, o administrador inicial é criado com `admin@conecta.local` e uma senha aleatória exibida uma única vez no terminal. Para gerar uma nova senha administrativa local, execute `npm run admin`.

### 2. Variáveis de ambiente (`.env`)

Copie `.env.example` para `.env` e preencha conforme o ambiente:

| Variável | Descrição |
|---|---|
| `PORT` | Porta do servidor local (padrão `3000`, escutando em `127.0.0.1`) |
| `DATABASE_URL` / `DATABASE_URL_UNPOOLED` | Conexão PostgreSQL Neon (pool e direta para migrações) |
| `ADZUNA_APP_ID` / `ADZUNA_APP_KEY` | Credenciais da API Adzuna para sincronizar vagas reais de estágio/aprendiz |
| `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD` | Envio de e-mails (recuperação de senha, autorização de responsável e alertas) |
| `APP_PUBLIC_URL` | URL pública da aplicação para links enviados por e-mail |
| `BACKUP_ENCRYPTION_KEY` | Chave hexadecimal de 64 caracteres (32 bytes) para cifrar backups AES-256-GCM |
| `CRON_SECRET` | Token de autorização para as rotas agendadas `/api/cron/sync` e `/api/cron/operations` |

### 3. Testes e manutenção

```bash
npm test              # Executa todas as suítes isoladas (API, regras, navegador e acessibilidade)
npm run migrate       # Aplica as migrações SQL no PostgreSQL configurado
npm run check-db      # Verifica conexão e contagens do banco ativo
npm run backup        # Gera e valida snapshot consistente (cifrado se houver chave)
npm run monitor       # Verifica a disponibilidade pública da aplicação
```

## 🔌 API (resumo)

Todas as rotas protegidas exigem o cookie de sessão `cj_session` (`HttpOnly`).

| Área | Método e rota | Descrição |
|---|---|---|
| Autenticação | `POST /api/auth/register` · `POST /api/auth/login` · `POST /api/logout` | Cadastro, login e encerramento de sessão |
| Recuperação e Idade | `POST /api/auth/recover` · `POST /api/auth/reset` · `POST /api/auth/guardian-confirm` | Redefinição de senha e confirmação do responsável |
| Perfil e Privacidade | `GET /api/me` · `PUT /api/profile` · `PUT /api/profile/preferences` · `GET /api/account/export` · `DELETE /api/account` | Dados do perfil, visibilidade, exportação e exclusão |
| Vagas e Favoritos | `GET /api/jobs` · `GET /api/external/jobs` · `POST /api/favorites/:id` · `POST /api/external/favorites/:id` | Listagem interna/externa e favoritos |
| Candidaturas | `POST /api/jobs/:id/apply` · `GET /api/applications` · `PATCH /api/applications/:id` | Candidatar-se, acompanhar etapas, avançar ou cancelar |
| Empresa | `PUT /api/company` · `POST /api/company/verification` · `GET/POST/PATCH /api/company/recruiters` · `GET/POST/PUT/PATCH /api/company/jobs` | Dados da empresa, aprovação, equipe e vagas |
| Notificações e Alertas | `GET /api/notifications` · `POST /api/notifications/read` · `GET/PUT /api/alerts` | Avisos internos e assinatura de alertas por e-mail |
| Administração e Saúde | `GET /api/admin` · `POST /api/admin/action` · `GET /api/admin/operations` · `GET /api/health` | Moderação, auditoria, operação e *healthcheck* |

## ⚙️ Configuração operacional

No `vercel.json` e nas rotinas de operação:

| Recurso | Funcionamento |
|---|---|
| `GET /api/cron/sync` | Agendado diariamente às `09:00 UTC` (`06:00` em São Paulo); limpa sessões/tokens expirados, sincroniza anúncios da Adzuna e envia lotes de alertas |
| `GET /api/cron/operations` | Agendado diariamente às `09:15 UTC` (`06:15` em São Paulo); executa backup diário cifrado em transação `REPEATABLE READ` e aplica retenção de 7 dias |
| `npm run monitor -- --watch` | Verifica `/api/health` a cada 60 segundos e dispara alertas por e-mail em caso de falha ou recuperação |

## 🩺 Problemas comuns

| Sintoma | Solução |
|---|---|
| Sem vagas externas no catálogo | Verifique se `ADZUNA_APP_ID` e `ADZUNA_APP_KEY` estão preenchidos no `.env` ou execute a sincronização manual no painel `/admin` |
| E-mail de recuperação ou responsável não enviado | Em ambiente local sem SMTP configurado, o link de recuperação é exibido diretamente no terminal do servidor |
| Empresa não consegue publicar vaga | Acesse `/admin` com uma conta administrativa e aprove a solicitação de verificação da empresa |
| Erro ao rodar testes de navegador | Instale o Chromium do Playwright com `npx playwright install chromium` |
| Estudante menor de 18 anos bloqueado ao candidatar | Confirme o link enviado ao e-mail do responsável cadastrado antes de enviar candidaturas |

## 🚧 Limitações conhecidas

- Chat interno entre candidato e recrutador, assinatura de contratos, pagamento de bolsas e agenda integrada de entrevistas estão fora do escopo desta entrega (MVP).
- Os anúncios externos importados da Adzuna redirecionam para a candidatura no site de origem, sem acompanhamento de etapas dentro da plataforma.
- O envio de alertas por e-mail é processado em lotes para respeitar o tempo limite das funções *serverless*, não havendo garantia de entrega instantânea.
- O piloto de avaliação com participantes reais possui protocolo preparado em [`docs/PILOTO.md`](docs/PILOTO.md), aguardando execução com usuários externos.

## 🗺️ Próximos passos sugeridos

- Realização do piloto presencial/remoto com estudantes e recrutadores para definição das metas finais de usabilidade
- Chat direto vinculado ao processo seletivo com mecanismos de bloqueio e denúncia
- Agenda integrada de entrevistas e exportação do perfil em currículo formatado (PDF)
- Integração com instituições de ensino para validação de convênios de estágio
```
