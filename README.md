# DNA Econômico — Da Essência à Decisão

> Dashboard de Controladoria Estratégica · Elder Sousa

---

## O que é este projeto?

Este é um sistema de análise financeira construído para acompanhar os resultados econômicos de um grupo empresarial. Ele lê arquivos CSV exportados do sistema de controladoria e transforma os números em visualizações interativas.

**Módulos planejados:**
- ✅ Financeiro (Fluxo de Caixa) — em construção
- 🔲 DRE (Demonstração do Resultado do Exercício)
- 🔲 Balanço Patrimonial
- 🔲 Planejamento e Orçamento (fase futura)

---

## Estrutura de pastas

```
controladoria-es/
├── index.html              ← Dashboard principal (não editar)
├── README.md               ← Este arquivo de documentação
└── data/
    ├── operacional/        ← CSVs da empresa operacional
    │   ├── FC_jan_2025.csv
    │   ├── FC_fev_2025.csv
    │   └── ...
    └── holding/            ← CSVs da holding
        ├── S FC_jan_2025.csv
        ├── S FC_fev_2025.csv
        └── ...
```

---

## Como adicionar um mês novo

### Passo 1 — Fazer upload do arquivo CSV

1. Acesse o repositório no GitHub
2. Entre na pasta `data/operacional/` ou `data/holding/`
3. Clique em **"Add file"** → **"Upload files"**
4. Arraste o arquivo CSV do mês novo
5. Clique em **"Commit changes"**

### Passo 2 — Registrar o arquivo no sistema

1. Abra o arquivo `index.html` no GitHub (clique no arquivo, depois no ícone de lápis ✏️)
2. Procure pela seção `ARQUIVOS` (por volta da linha 120)
3. Adicione o nome do novo arquivo na lista correta:

```javascript
// Exemplo: adicionando janeiro de 2026
operacional: [
  'FC_jan_2025.csv',
  // ... outros meses ...
  'FC_dez_2025.csv',
  'FC_jan_2026.csv',  // ← adicione aqui
],
```

4. Clique em **"Commit changes"**

Pronto! O novo mês já aparece no dashboard.

---

## Padrão de nomes dos arquivos

| Empresa | Formato | Exemplo |
|---------|---------|---------|
| Operacional | `FC_mmm_aaaa.csv` | `FC_jan_2025.csv` |
| Holding | `S FC_mmm_aaaa.csv` | `S FC_jan_2025.csv` |

**Meses abreviados:** jan, fev, mar, abr, mai, jun, jul, ago, set, out, nov, dez

---

## Como funciona o sistema (para aprendizado)

### O que é HTML, CSS e JavaScript?

Pense no dashboard como uma loja física:

- **HTML** = a estrutura física da loja (paredes, prateleiras, balcão)
- **CSS** = a decoração (cores, fontes, espaçamentos, o visual)
- **JavaScript** = os funcionários (a lógica, o que acontece quando você clica em algo)

### O que é um CSV?

CSV significa *Comma-Separated Values* (Valores Separados por Vírgula). É um arquivo de texto simples onde cada linha é um registro e os valores são separados por ponto-e-vírgula ou vírgula. É como uma planilha Excel, mas sem formatação — só os dados puros.

### O que é o GitHub?

O GitHub é um serviço que guarda o código do projeto na nuvem, com histórico de todas as alterações. É como um Google Drive especializado para código, que registra cada mudança feita, quando foi feita e por quem.

### O que é o GitHub Pages?

É um serviço gratuito do GitHub que transforma um repositório em um site. Quando você ativa o GitHub Pages, o arquivo `index.html` vira uma página acessível por qualquer pessoa via URL.

### O que é `fetch()`?

É uma função do JavaScript que busca um arquivo na internet. Quando o dashboard carrega, ele usa `fetch()` para buscar os arquivos CSV do GitHub. É como um funcionário que vai ao depósito buscar um produto quando o cliente pede.

### O que é `async/await`?

Quando o JavaScript busca um arquivo na internet, isso leva tempo. O `async/await` diz ao sistema: "espera a resposta antes de continuar". Sem isso, o código tentaria usar os dados antes de eles chegarem.

### O que é "cache"?

Cache é uma memória temporária. No nosso sistema, quando um CSV é carregado pela primeira vez, ele fica salvo na memória do navegador. Se o usuário trocar de filtro e precisar do mesmo arquivo de novo, o sistema usa o que já está na memória em vez de buscar de novo — é mais rápido.

---

## Histórico de versões

| Versão | Data | O que mudou |
|--------|------|-------------|
| 1.0 | Abr/2026 | Estrutura inicial · Módulo Financeiro Página 1 |

---

## Próximos passos

- [ ] Upload dos CSVs operacional para pasta `data/operacional/`
- [ ] Upload dos CSVs holding para pasta `data/holding/`
- [ ] Ativar GitHub Pages
- [ ] Implementar Página 2 (Comparação de Períodos)
- [ ] Implementar consolidado (Operacional + Holding)
- [ ] Criar módulo DRE

---

*Projeto desenvolvido com Claude AI · github.com/elder901/controladoria-es*
