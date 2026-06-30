# Gestão de Contratos — Prefeitura de Oriximiná/PA

Sistema web de gestão e monitoramento de contratos públicos da Prefeitura Municipal de Oriximiná, com alertas de vencimento, análise contratual e exportação de dados.

---

## Funcionalidades

### Dashboard
- Cards com totais: contratos ativos, críticos (≤20 dias), em aviso (21–30 dias), expirados e valor global
- 3 gráficos interativos: contratos por situação, por mês de vencimento e evolução de valores por ano
- Banner de alerta automático quando há contratos em estado crítico

### Contratos
- Tabela completa com 13 colunas (processo, contrato, fiscal, contratada, CNPJ/CPF, objeto, órgão, modalidade, vigência, valor, situação, ações)
- Busca em tempo real em todos os campos
- Filtros por situação e por prazo de vencimento
- Paginação (12 registros por página)
- Cadastrar, editar, visualizar e excluir contratos
- Relatório de impressão formatado

### Alertas
- Seções separadas: contratos críticos, em aviso e expirados
- Ações rápidas: gerar notificação, abrir análise, arquivar, ver análise existente
- Contagem de dias restantes com código de cores

### Relatórios
- Cards estatísticos: aditivados, finalizados, suspensos e valor médio
- Gráfico de contratos por secretaria/setor
- Ranking top 10 contratos por valor

### Exportação
- Filtros por situação, mês de vencimento e urgência
- Exportar como **CSV** (com BOM para Excel) ou **HTML** imprimível

### Contratos Arquivados
- Listagem de contratos arquivados com link para análise
- Opção de desarquivar

### Análise Contratual
- Formulário para decisão (Renovar / Encerrar) com justificativa
- Upload de PDF para Firebase Storage
- Visualização da análise salva com acesso ao arquivo

### Integrações
- **1Doc** — acesso direto à plataforma de gestão documental da prefeitura via atalho na sidebar

---

## Tecnologias

| Camada | Tecnologia |
|---|---|
| Marcação | HTML5 |
| Estilo | CSS3 com variáveis CSS (design system próprio) |
| Lógica | JavaScript ES6+ (módulos, async/await) |
| Ícones | Bootstrap Icons 1.11.3 |
| Gráficos | Chart.js (CDN) |
| Fontes | Sora + JetBrains Mono (Google Fonts) |
| Autenticação | Firebase Authentication (email/senha) |
| Banco de dados | Cloud Firestore |
| Armazenamento | Firebase Storage (PDFs de análise) |
| Pacotes | Bootstrap 5.3.8, Firebase 12.9.0 (npm) |

---

## Estrutura do Projeto

```
gest-o-de-contratos-main/
├── index.html          # Estrutura HTML completa (SPA)
├── script.js           # Toda a lógica da aplicação
├── style.css           # Design system e estilos
├── package.json        # Dependências npm
├── img/
│   ├── prefeitura.png  # Brasão da prefeitura
│   └── 1doc.png        # Logo do 1Doc
└── node_modules/       # Dependências instaladas
```

---

## Banco de Dados (Firestore)

### Coleção `contratos`

| Campo | Tipo | Descrição |
|---|---|---|
| `processo` | string | Número do processo |
| `contrato` | string | Número do contrato |
| `responsavel` | string | Fiscal responsável |
| `contratada` | string | Nome da empresa/pessoa |
| `cnpj` | string | CNPJ ou CPF |
| `objeto` | string | Objeto do contrato |
| `setor` | string | Secretaria/órgão |
| `modalidade` | string | Modalidade de licitação |
| `vigInicial` | string | Data de início (YYYY-MM-DD) |
| `vigFinal` | string | Data de vencimento (YYYY-MM-DD) |
| `valorGlobal` | number | Valor do contrato em R$ |
| `situacao` | string | Ativo / Finalizado / Renovação / Aditivado / Suspenso |
| `arquivado` | boolean | Se está arquivado |
| `userId` | string | UID do usuário que cadastrou |
| `createdAt` | timestamp | Data de criação |
| `updatedAt` | timestamp | Data da última alteração |

### Coleção `analises`

| Campo | Tipo | Descrição |
|---|---|---|
| `contratoId` | string | ID do contrato relacionado |
| `decisao` | string | Renovar ou Encerrar |
| `justificativa` | string | Texto da justificativa |
| `arquivo` | string | URL do PDF no Storage |
| `status` | string | Status da análise |
| `criadoEm` | timestamp | Data de criação |
| `criadoPor` | string | UID do usuário |

---

## Instalação e Execução

### Pré-requisitos
- Node.js 18+ e npm
- Conta no [Firebase](https://firebase.google.com) com projeto configurado

### Passos

```bash
# Clone o repositório
git clone https://github.com/HectorCardoso53/gest-o-de-contratos.git
cd gest-o-de-contratos

# Instale as dependências
npm install
```

Abra o `index.html` diretamente no navegador ou use uma extensão de servidor local (ex: Live Server no VS Code).

### Configuração do Firebase

As credenciais do Firebase estão no início do `script.js`:

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "gestao-de-contratos-4ad66.firebaseapp.com",
  projectId: "gestao-de-contratos-4ad66",
  storageBucket: "gestao-de-contratos-4ad66.firebasestorage.app",
  messagingSenderId: "528807844859",
  appId: "1:528807844859:web:2fcfae3f1e6f1e51bec95e"
};
```

Para rodar em outro projeto Firebase, substitua esses valores pelas credenciais do seu projeto e ative os serviços **Authentication**, **Firestore** e **Storage** no console.

---

## Principais Funções

| Função | Descrição |
|---|---|
| `logar()` | Autentica o usuário via Firebase |
| `logout()` | Encerra a sessão |
| `initApp()` | Carrega dados e inicializa a aplicação |
| `goTo(page)` | Navega entre as páginas (roteamento SPA) |
| `renderDashboard()` | Renderiza cards e gráficos do dashboard |
| `renderTabela()` | Renderiza tabela de contratos com filtros e paginação |
| `renderAlertas()` | Exibe contratos por urgência de vencimento |
| `renderRelatorios()` | Gera estatísticas e gráficos de relatório |
| `salvarContrato()` | Cria ou atualiza contrato no Firestore |
| `excluirContrato(id)` | Remove contrato com confirmação |
| `salvarAnalise()` | Salva análise com upload de PDF |
| `arquivarContrato(id)` | Arquiva contrato |
| `exportarCSV()` | Gera e baixa arquivo CSV |
| `diasRestantes(data)` | Calcula dias até o vencimento |
| `toast(msg, type)` | Exibe notificação na tela |

---

## Autenticação

O sistema usa **Firebase Authentication** com e-mail e senha institucional (`usuario@oriximina.pa.gov.br`). A sessão é mantida por `browserSessionPersistence` — ao fechar o navegador, o usuário precisa logar novamente.

O nome exibido na sidebar é extraído automaticamente do e-mail (parte antes do `@`), formatado com iniciais em maiúsculo.

---

## Desenvolvido por

**Secretaria Municipal de Eficiência Governamental — SEMEG**  
Prefeitura Municipal de Oriximiná/PA
