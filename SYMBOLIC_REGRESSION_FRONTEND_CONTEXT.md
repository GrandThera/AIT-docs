# Grand Thera Symbolic Regression Frontend Context

Este documento deve ser usado como contexto para continuar ou recriar o dashboard `Symbolic Regression Workbench` em novos chats. O objetivo e preservar a identidade visual Grand Thera, o estilo operacional Palantir-like e as decisoes de UX ja aplicadas no dashboard.

## Produto

- **Nome da experiencia:** Symbolic Regression Workbench.
- **Arquivo atual:** `dashboards/symbolic_regression_dashboard.html`.
- **Dominio:** ferramenta analitica institucional para regressao simbolica, modelagem estatistica e exploracao interativa de variaveis.
- **Publico:** usuarios tecnicos, quant, analistas financeiros, ciencia de dados e pesquisa aplicada.
- **Tom:** preciso, denso, operacional, moderno, sem aparencia de landing page.

## Principio Visual

A interface deve parecer uma ferramenta de investigacao analitica:

- paineis escuros com bordas finas;
- informacao densa, organizada e escaneavel;
- controles discretos;
- pouco texto explicativo direto;
- tooltips/modais para contexto;
- tabelas e graficos com fullscreen;
- visual institucional, sem decoracao gratuita;
- nenhuma hero section, card decorativo ou bloco de marketing.

## Tema E Tokens

Use os mesmos tokens do contexto Grand Thera:

```css
:root {
  color-scheme: dark;
  --bg: #07090d;
  --panel: #10141b;
  --panel-2: #151a23;
  --ink: #f5f7fb;
  --muted: #9aa4b2;
  --line: #293241;
  --soft: #1b2230;
  --accent: #7dd3fc;
  --accent-2: #5eead4;
  --danger: #f05252;
  --warn: #d6a93a;
  --good: #2fb37c;
  --shadow: 0 18px 50px rgba(0, 0, 0, 0.35);
}

html[data-theme="light"] {
  color-scheme: light;
  --bg: #f5f7fb;
  --panel: #ffffff;
  --panel-2: #f9fbff;
  --ink: #121826;
  --muted: #667085;
  --line: #d8deea;
  --soft: #edf1f7;
  --accent: #155eef;
  --accent-2: #0f766e;
  --danger: #b42318;
  --warn: #a15c07;
  --good: #067647;
  --shadow: 0 18px 42px rgba(16, 24, 40, 0.10);
}
```

Regras:

- Dark mode e o padrao inicial.
- O botao de tema deve mostrar a **acao disponivel**, nao o tema atual: sol quando esta em dark, lua quando esta em light.
- Evite paletas muito coloridas. Azul/ciano deve dominar. Vermelho, amarelo e verde ficam para erro, alerta e sucesso.

## Tipografia

Use:

```css
font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
```

Regras:

- Nao usar letter spacing negativo.
- Titulos de painel: 14px a 16px, peso 700.
- Numeros de metric cards podem ser maiores, mas sem escala de hero.
- Texto auxiliar sempre em `--muted`.
- Formula matematica renderizada em PNG/canvas deve usar fonte matematica como `Cambria Math` ou `Times New Roman`.

## Layout

Use largura maxima:

```css
main,
.topbar {
  width: min(1440px, 100%);
  margin: 0 auto;
}
```

Estrutura recomendada:

- Header sticky compacto.
- Linha de metric cards no topo.
- Grid principal com painel grande a esquerda e controles a direita.
- Secoes inferiores em `split` de duas colunas.
- Tabelas e paineis importantes com botao de fullscreen.

```css
.layout {
  display: grid;
  grid-template-columns: minmax(0, 1.65fr) minmax(360px, 0.9fr);
  gap: 16px;
}

.split {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 16px;
}
```

Mobile:

```css
@media (max-width: 980px) {
  .layout,
  .split {
    grid-template-columns: 1fr;
  }
}
```

## Header

O header deve conter:

- logo Grand Thera;
- titulo curto;
- upload CSV/XLS;
- sample data;
- export JSON;
- command palette `Ctrl K`;
- toggle de tema por icone.

Nao colocar descricoes longas. O header e operacional.

## Logos

Use:

- `assets/grand-thera-logo-white.png` no dark mode;
- `assets/grand-thera-logo-primary.png` no light mode.

```css
html[data-theme="dark"] .logo-light { display: none; }
html[data-theme="light"] .logo-dark { display: none; }
```

## Paineis

Todo painel analitico deve ter:

- titulo curto;
- botao `i` para tooltip/modal;
- botao fullscreen quando houver grafico ou tabela relevante;
- corpo sem padding excessivo;
- borda fina `--line`;
- background `--panel`/`--panel-2`.

Nao colocar card dentro de card.

## Icones De Tooltip E Fullscreen

Os botoes devem seguir o estilo compacto do dashboard:

- `i` circular pequeno;
- fullscreen quadrado pequeno com cantos desenhados;
- hover com borda/acento discreto;
- foco visivel.

O fullscreen deve preservar o tema e expandir graficos/tabelas para usar a altura disponivel.

## Componentes Obrigatorios Do Dashboard

Para a aplicacao de regressao simbolica, manter:

- `Data Source`: upload CSV/XLS/XLSX/JSON e fetch por API.
- seletor de variavel dependente;
- checkboxes para variaveis independentes;
- seletor de max terms;
- botao `Fit Symbolic Model`;
- `Interactive Scenario` com sliders e valor visivel ao lado;
- metric cards: Rows, Variables, R2, RMSE, BIC, Prediction;
- `Actual vs Symbolic Fit`;
- `LaTeX Formula` com texto LaTeX e imagem PNG da formula matematica final;
- `Observed vs Predicted`;
- `Residual Diagnostics`;
- `Statistical Summary`;
- `Dependence Diagnostics` com Pearson e mutual information;
- `Data Preview`.

## Formula Matematica

O painel `LaTeX Formula` deve mostrar duas representacoes:

1. Texto LaTeX da expressao.
2. PNG/canvas da formula final renderizada como matematica, nao como codigo LaTeX cru.

Regras:

- Nao exibir `\hat{...}` ou `z_{...}` cru no PNG.
- O PNG deve parecer formula matematica: alvo com chapeu, expoentes sobrescritos, funcoes `sin`, `cos`, `log`, raiz e fracoes/razoes legiveis.
- A legenda pode ser curta: `PNG render of final mathematical expression`.

## Graficos

Todos os graficos em canvas devem:

- responder a hover;
- exibir tooltip com linha/observacao e valores;
- ter legenda ou labels de eixo quando relevante;
- manter paleta fria;
- redesenhar em resize e troca de tema.

Regras por grafico:

- `Actual vs Symbolic Fit`: linha real vs linha prevista; hover por observacao.
- `Observed vs Predicted`: scatter com diagonal; labels `Observed` e `Predicted`; hover destaca ponto.
- `Residual Diagnostics`: residual por observacao; linha zero; labels `Observation` e `Residual`; hover destaca datapoint.

## Tabelas

Tabelas devem ser compactas:

- fonte 12px;
- header sticky;
- bordas horizontais finas;
- valores numericos alinhados a direita;
- primeira coluna alinhada a esquerda;
- fullscreen em tabelas relevantes.

Paineis com fullscreen obrigatorio:

- Statistical Summary;
- Dependence Diagnostics;
- Data Preview;
- graficos principais.

## Metricas Estatisticas

`Statistical Summary` deve incluir:

- Observations;
- Parameters;
- R2;
- Adjusted R2;
- RMSE;
- MAE;
- MAPE;
- RSS;
- AIC;
- BIC.

Tooltip recomendado:

`Observations: rows used. Parameters: intercept plus selected symbolic terms. R2: explained variance. Adjusted R2: R2 penalized by complexity. RMSE: typical error magnitude. MAE: average absolute error. MAPE: average percentage error. RSS: residual sum of squares. AIC/BIC: lower is better after complexity penalty.`

## Dependence Diagnostics

Adicionar e manter diagnosticos de dependencia entre a variavel dependente e cada variavel independente selecionada:

- Pearson r;
- valor absoluto de Pearson;
- mutual information;
- normalized mutual information;
- numero de bins.

Restricao: calcular tudo na mao, sem bibliotecas.

Pearson:

- usar media, covariancia e desvios padrao.

Mutual information:

- discretizar `x` e `y` por bins de quantis;
- montar tabela conjunta;
- calcular `sum p(x,y) * log(p(x,y)/(p(x)p(y)))`;
- normalizar por `sqrt(Hx * Hy)`.

## Motor Analitico

O dashboard deve continuar sem dependencias de terceiros.

Regras:

- Parser CSV manual com suporte a delimitadores comuns.
- XLS textual/HTML/XML spreadsheet manual.
- XLSX pode usar `DecompressionStream` nativo do browser, sem lib externa.
- Tratamento de dados: conversao numerica robusta, imputacao por mediana, winsorizacao p01/p99, z-score.
- Regressao simbolica manual:
  - gerar candidatos: `z(x)`, quadrado, cubo, raiz, log, sin, cos, interacoes e razoes;
  - selecionar termos por criterio BIC de forma gulosa;
  - resolver OLS por algebra linear propria, como eliminacao gaussiana;
  - nao usar sklearn, math.js, d3, chart.js ou bibliotecas similares.

## Interactive Scenario

O painel deve:

- listar variaveis independentes selecionadas;
- ter slider por variavel;
- mostrar o valor atual ao lado do slider em box fixo;
- recalcular a predicao imediatamente;
- mostrar output da formula e baseline por medianas.

## Command Palette

Manter `Ctrl K` com acoes como:

- Fit symbolic model;
- Toggle theme;
- Load sample data;
- Export model JSON;
- Reset scenario to medians;
- Jump to residual diagnostics.

## Copywriting

Usar labels curtos em ingles:

- Rows;
- Variables;
- Prediction;
- Actual vs Symbolic Fit;
- LaTeX Formula;
- Interactive Scenario;
- Observed vs Predicted;
- Residual Diagnostics;
- Statistical Summary;
- Dependence Diagnostics;
- Data Preview.

Explicacoes ficam em tooltips/modais, nao no corpo da tela.

## Checklist De Continuidade

Ao continuar em novo chat:

1. Abrir `docs/FRONTEND_CONTEXT.md`.
2. Abrir este arquivo.
3. Abrir `dashboards/symbolic_regression_dashboard.html`.
4. Manter alteracoes autocontidas no HTML salvo em `dashboards/`.
5. Validar o JS inline com Node:

```powershell
node -e "const fs=require('fs'); const html=fs.readFileSync('dashboards/symbolic_regression_dashboard.html','utf8'); const m=html.match(/<script>([\s\S]*)<\/script>/); new Function(m[1]); console.log('script-ok');"
```

6. Recarregar o in-app browser em:

```text
file:///C:/Projetos/GITHUB%20GT/dashboards/symbolic_regression_dashboard.html
```

## Observacao Sobre Versionamento

O diretorio `dashboards/` esta no `.gitignore`. O dashboard e tratado como artefato local gerado, a menos que o usuario solicite explicitamente alterar o versionamento.
