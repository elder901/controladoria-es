# DNA Econômico — Da Essência à Decisão

> Dashboard de Controladoria Estratégica · Elder Sousa  
> Grupo Empresarial Tejotão + Holding Sena

---

## O que é este projeto?

Um sistema completo de análise financeira que lê arquivos exportados do sistema de controladoria e transforma os números em visualizações interativas. Tudo funciona em um único arquivo `index.html` hospedado gratuitamente no GitHub Pages — sem servidor, sem banco de dados, sem custo.

**Acesse:** https://elder901.github.io/controladoria-es

---

## Módulos disponíveis

| Módulo | Status | O que faz |
|--------|--------|-----------|
| Financeiro | ✅ Ativo | Análise do Fluxo de Caixa — DRE Financeira mês a mês e comparação de períodos |
| DRE | ✅ Ativo | Demonstração do Resultado — regime de competência, A.V.% e Δ p.p. |
| Balanço Patrimonial | ✅ Ativo | Evolução mensal, índices de liquidez, endividamento e rentabilidade |
| Balancete | ✅ Ativo | Consulta por conta contábil, filtro por unidade e nível hierárquico |
| Inteligência | ✅ Ativo | Cruzamento FC + DRE + BP — bridge lucro→caixa, ciclo financeiro, ROE, ROA |
| Previsão | ✅ Ativo | Simulador de faturamento e projeção de 12 meses |

---

## Estrutura de pastas

```
controladoria-es/
├── index.html                  ← Dashboard completo (toda a lógica está aqui)
├── README.md                   ← Este guia
└── data/
    ├── operacional/            ← FC do Tejotão
    │   ├── FC jan 2025.csv
    │   └── ...
    ├── holding/                ← FC da Sena
    │   ├── S FC jan 2025.csv
    │   └── ...
    ├── dre/                    ← DRE econômica
    │   ├── DREjan25.csv
    │   └── ...
    ├── bp/                     ← Balanço Patrimonial
    │   ├── BPjan25.csv
    │   └── ...
    └── balancete/              ← Balancete contábil por unidade
        ├── bal0125.Html
        └── ...
```

---

## Padrão de nomes dos arquivos

| Módulo | Empresa | Formato | Exemplo |
|--------|---------|---------|---------|
| Financeiro | Tejotão | `FC mmm aaaa.csv` | `FC jan 2025.csv` |
| Financeiro | Sena | `S FC mmm aaaa.csv` | `S FC jan 2025.csv` |
| DRE | Ambas | `DREmmmAA.csv` | `DREjan25.csv` |
| Balanço | Tejotão | `BPmmmAA.csv` | `BPjan25.csv` |
| Balancete | Tejotão | `balmm AA.Html` | `bal0125.Html` |

**Meses abreviados:** jan, fev, mar, abr, mai, jun, jul, ago, set, out, nov, dez

---

## Como adicionar um mês novo

### Passo 1 — Subir o arquivo CSV

**Arquivos pequenos (abaixo de 25MB) — pelo editor web:**
1. Acesse o repositório no GitHub
2. Entre na pasta correta (ex: `data/operacional/`)
3. Clique em **"Add file"** → **"Upload files"**
4. Arraste o arquivo CSV → **"Commit changes"**

**Arquivos grandes (balancete HTML) — pelo GitHub Desktop:**
1. Abra o GitHub Desktop → **"Show in Explorer"**
2. Copie o arquivo para a pasta correta
3. No GitHub Desktop: escreva uma mensagem → **"Commit to main"** → **"Push origin"**

### Passo 2 — Registrar o arquivo no index.html

1. Abra o `index.html` no GitHub (ícone de lápis ✏️)
2. Use **Ctrl+F** para localizar `ARQ`
3. Adicione o nome do novo arquivo na lista:

```javascript
operacional: [
  'FC jan 2025.csv',
  // ... outros meses ...
  'FC mar 2026.csv',
  'FC abr 2026.csv',  // ← adicione aqui
],
```

4. Clique em **"Commit changes"**

---

## Encodings dos arquivos

| Arquivo | Encoding | Por quê |
|---------|----------|---------|
| FC, DRE, BP | `latin-1` | Exportado pelo sistema de gestão |
| Balancete | `windows-1252` | Exportado pelo Windows em HTML |

> **O que é encoding?** É o "idioma" que o computador usa para entender caracteres especiais como ã, ç, ê. Se errado, os acentos aparecem como símbolos estranhos.

---

## Fórmulas financeiras validadas

### Ciclo Financeiro

```
PME = Estoques ÷ (CMV ÷ 30)
PMR = Contas a Receber ÷ (Receita Bruta ÷ 30)
PMP = Fornecedores ÷ (Compras ÷ 30)
      onde Compras = CMV + Estoque Final - Estoque Inicial

Ciclo Financeiro = PME + PMR - PMP
```

### Bridge Lucro → Caixa (precisão validada: 99,6%)

```
(+) Lucro Líquido Final (DRE)
    Camada 1 — Capital de Giro:
    (+/-) Clientes, Fornecedores, Estoques
    Camada 2 — Investimentos:
    (+/-) Imobilizado (CAPEX), Financiamentos
    Camada 3 — Outros passivos:
    (+/-) Salários, Obrigações Fiscais, Outras Obrigações
= Geração de Caixa Estimada
```

### Parâmetros históricos (Tejotão — base jan/25 a mar/26)

```
CMV:                  72,7% da Receita Líquida
Despesas Variáveis:    4,7% da Receita Líquida
Deduções:              8,2% da Receita Bruta
Despesas Fixas:       ~R$ 2.208.705/mês
Recebimento imediato:  61,9% do faturamento (mesmo mês)
Recebimento diferido:  38,1% do faturamento (mês seguinte)
```

---

## Como o sistema funciona (guia de aprendizado)

### A analogia da loja

- **HTML** = estrutura física (paredes, prateleiras, balcão)
- **CSS** = decoração (cores, fontes, visual)
- **JavaScript** = funcionários (lógica, o que acontece ao clicar)

### O fluxo de uma ação

```
Clique no botão "DRE"
        ↓
S.modulo = 'dre'  ← estado muda
        ↓
renderDRE() é chamada
        ↓
loadDRE() busca os CSVs no GitHub via fetch()
        ↓
parseDRE() lê linha por linha e extrai números
        ↓
JS monta o HTML da tabela como texto
        ↓
document.getElementById('main').innerHTML = tabela
        ↓
Você vê a tabela na tela
```

### O objeto de estado S

```javascript
S = {
  modulo: 'financeiro',  // qual módulo está ativo
  emp:    'operacional', // Operacional, Holding ou Consolidado
  pag:    1,             // Página 1 ou 2
  ini:    'jan/25',      // início do período selecionado
  fim:    'mar/26',      // fim do período selecionado
  cache:  {}             // arquivos já carregados (evita buscar de novo)
}
```

Quando qualquer coisa muda, o JS redesenha a tela inteira com os novos valores.

### Por que parser por posição de linha?

Os arquivos CSV do sistema sempre exportam na mesma estrutura — a linha 10 do FC **sempre** é o Total de Vendas, a linha 64 **sempre** é a Variação de Caixa. Ler por posição é mais robusto do que buscar por nome, que pode quebrar com problemas de encoding.

---

## Arquitetura das fontes de dados

| Campo | Fonte | Detalhe |
|-------|-------|---------|
| Fluxo de Caixa | FC CSV | Col 3 (valor), leitura por linha |
| DRE Tejotão | DRE CSV | Col 6 (período atual) |
| DRE Sena | DRE CSV | Col 12 |
| DRE Consolidada | DRE CSV | Col 14 (já elimina inter-companhia) |
| BP Atual | BP CSV | Col 5 |
| BP Anterior | BP CSV | Col 6 (usado no bridge e no PME/PMP) |
| Balancete Total | Balancete HTML | Cols 4-9 |
| Balancete por Unidade | Balancete HTML | MG:10-15, MV:16-21, AM:22-27, IL:28-33, IND:34-39 |

---

## Unidades do Tejotão

| Código | Loja | Status |
|--------|------|--------|
| 001 | Mato Grosso | ✅ Operando |
| 002 | Melo Viana | ✅ Operando |
| 003 | Amazonas | ✅ Operando |
| 004 | Interlagos | 🔲 Não iniciou |
| 005 | Indústria | 🔲 Não iniciou |

---

## Problemas comuns e soluções

| Problema | Causa provável | Solução |
|----------|---------------|---------|
| Dashboard em branco | Erro de JavaScript | F12 → Console → verificar o erro |
| Acentos quebrados | Encoding errado | Verificar se o arquivo usa latin-1 ou windows-1252 |
| Dados não aparecem | Arquivo não registrado | Adicionar nome na lista ARQ do index.html |
| Arquivo não sobe | Arquivo muito grande | Usar GitHub Desktop |
| Dados desatualizados | Cache do browser | Abrir em aba anônima (Ctrl+Shift+N) |
| "X is not defined" | Erro de JS anterior | Verificar console — há outro erro antes |

---

## Histórico de versões

| Versão | Data | O que mudou |
|--------|------|-------------|
| v1.0 | Abr/2025 | Estrutura inicial · Módulo Financeiro básico |
| v2.0 | Mai/2026 | 6 módulos completos · Bridge validado 99,6% · Ciclo financeiro correto · A.V.% e Δ p.p. · Análise por unidade · Balancete com hierarquia · Módulo Inteligência com cruzamento FC+DRE+BP |

---

## Próximos passos

- [ ] Orçamento formal — CSV de metas para comparar com realizado
- [ ] Enriquecimento com balancete (saldo por banco, resultado por unidade)
- [ ] PWA — dashboard instalável no Android
- [ ] App com login e níveis de usuário (fase futura)
- [ ] Salvar skill do Claude com contexto completo do projeto

---

## Glossário

| Termo | Significado simples |
|-------|---------------------|
| HTML | Define a estrutura da página |
| CSS | Define o visual (cores, tamanhos) |
| JavaScript | Define a lógica e o comportamento |
| CSV | Arquivo de texto com dados separados por ponto-e-vírgula |
| fetch() | Função JS que busca um arquivo na internet |
| async/await | Mecanismo para esperar a resposta antes de continuar |
| Cache | Memória temporária — evita baixar o mesmo arquivo duas vezes |
| CORS | Política de segurança do browser para requisições entre domínios |
| Encoding | Sistema de codificação de caracteres especiais (ã, ç, ê) |
| GitHub Pages | Serviço gratuito que transforma o repositório em site público |
| Parser | Código que lê e interpreta um arquivo linha por linha |
| Bridge | Reconciliação entre lucro (DRE) e caixa (FC) |
| PME / PMR / PMP | Prazos médios de estoque, recebimento e pagamento |
| A.V.% | Análise Vertical — percentual de cada linha sobre a Receita Líquida |
| Δ p.p. | Variação em pontos percentuais entre dois períodos |
| CAPEX | Capital Expenditure — investimento em ativo imobilizado |
| ROE | Retorno sobre Patrimônio Líquido |
| ROA | Retorno sobre Ativo Total |


Bloco 1 — HTML (o esqueleto)
Pensa no HTML como o formulário em branco. Ele define o que existe na página, mas não como parece nem o que faz. São as linhas ~110 a 125 do seu arquivo — bem pequeno comparado ao resto.
Exemplo real do seu código:
html<div class="header">        ← "existe um cabeçalho aqui"
  <div class="header-title">DNA de PJ</div>   ← "existe um título aqui"
</div>
<div id="main"></div>       ← "existe uma área de conteúdo aqui" (começa vazia!)
Repara no id="main" — ele começa completamente vazio. Quem preenche ele é o JavaScript, não o HTML. Isso é importante.

Bloco 2 — CSS (a aparência)
CSS é a roupa do esqueleto. Ele pega os elementos do HTML e define cor, tamanho, espaçamento.
Exemplo real do seu código:
css:root {
  --bg: #f5f7fb;        ← "a cor de fundo é esse azul clarinho"
  --accent: #1d4ed8;    ← "o azul principal é esse"
  --good: #15803d;      ← "verde = bom"
  --bad: #b91c1c;       ← "vermelho = ruim"
}
Isso se chama variáveis CSS. É por isso que mudar uma cor no topo afeta o sistema inteiro — todos os elementos usam var(--accent) em vez de repetir #1d4ed8 mil vezes.

Bloco 3 — JavaScript (o comportamento)
Esse é o cérebro — 97% do arquivo. Ele responde a cliques, busca os CSVs, calcula os indicadores e monta o HTML dentro do div#main.
A lógica central do seu sistema é essa:
Você clica em "DRE"
    → JavaScript chama renderDRE()
    → renderDRE() busca os CSVs (fetch)
    → parseia os números (parseDRE)
    → constrói o HTML da tabela como texto
    → injeta tudo dentro do div#main
    → você vê a tela

A grande sacada
O  sistema é um single-page app — uma única página que se reescreve sozinha conforme você clica. Não tem "página da DRE" e "página do BP". Tem uma página só com um div que troca de conteúdo.
Isso é exatamente o que frameworks modernos como React fazem — só que no seu caso foi feito na mão, em JavaScript puro. Você tem um sistema mais sofisticado do que imagina.

---

*Projeto desenvolvido com Claude AI (Anthropic)*  
*Elder Sousa — Controladoria Estratégica · Grupo Tejotão + Sena*  
*github.com/elder901/controladoria-es*s*
