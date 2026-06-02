# Grand Thera Frontend Context

Este documento descreve o contexto visual e funcional do dashboard `AIT Simple_RegimeDetection`, para reutilizacao do layout em outras aplicacoes Grand Thera.

## Objetivo De UX

A interface deve parecer uma ferramenta analitica institucional: densa, clara, operavel e moderna. A referencia visual e "Palantir-like": paineis escuros, bordas finas, alta hierarquia informacional, controles discretos, visualizacoes interativas e leitura rapida de sinais.

Evite aparencia de landing page, excesso de texto explicativo, cards decorativos ou espacos vazios. A primeira tela deve ser diretamente operacional.

## Personalidade Visual

- **Produto:** plataforma analitica financeira.
- **Tom:** tecnico, institucional, preciso.
- **Sensacao:** command center, monitoramento, diagnostico, investigacao.
- **Densidade:** media-alta, com pouca ornamentacao.
- **Interacao:** todo grafico relevante deve responder a hover, clique, zoom, selecao ou fullscreen.

## Temas

A aplicacao suporta `dark` e `light` mode via atributo:

```html
<html data-theme="dark">
```

### Dark Mode

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
  --shadow: 0 18px 50px rgba(0, 0, 0, 0.35);
}
```

### Light Mode

```css
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
  --shadow: 0 18px 42px rgba(16, 24, 40, 0.10);
}
```

## Cores De Regime

Use cores consistentes em todos os widgets: timeline, serie historica, resumo, heatmap e inspector.

```css
--regime-bull: #2fb37c;
--regime-bear: #f05252;
--regime-sideways: #d6a93a;
```

Para regimes adicionais, use tons frios, mantendo o dashboard mais azul e institucional:

```css
--regime-extra-1: #4f8cff;
--regime-extra-2: #38bdf8;
--regime-extra-3: #2dd4bf;
--regime-extra-4: #818cf8;
```

## Escala De Heatmap E Superficie

Para heatmaps quantitativos e superficie 3D, a escala deve ser fria e coerente: valor baixo em azul profundo, valor alto em azul claro/ciano.

```css
--scale-low: #08205f;
--scale-mid: #1e6fb8;
--scale-high: #7dd3fc;
```

Evite espectros muito quentes ou arco-iris quando o dashboard estiver em contexto institucional. Vermelho, amarelo e verde devem ficar reservados aos regimes ou a alertas especificos.

## Tipografia

Fonte recomendada:

```css
font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
```

Regras:

- Nao use letter spacing negativo.
- Use pesos 500-700 para titulos e numeros.
- Titulos de painel devem ficar entre 14px e 16px.
- Numeros principais em cards devem ter presenca, mas sem escala de hero.
- Texto auxiliar deve usar `--muted`.

## Estrutura Geral

Use largura maxima centralizada:

```css
main,
.topbar {
  width: min(1440px, 100%);
  margin: 0 auto;
}
```

Layout principal recomendado:

```css
.layout {
  display: grid;
  grid-template-columns: minmax(0, 1.7fr) minmax(360px, 0.9fr);
  gap: 16px;
  align-items: stretch;
  margin-top: 20px;
}
```

Para secoes inferiores:

```css
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

O header deve ser sticky, compacto e operacional:

- logo Grand Thera a esquerda;
- titulo curto do produto;
- seletor de modelo/K;
- toggle dark/light;
- botao de command palette;
- upload de CSV/XLS;
- exportacoes.

Padrao:

```css
header {
  position: sticky;
  top: 0;
  z-index: 2;
  background: color-mix(in srgb, var(--panel) 88%, transparent);
  border-bottom: 1px solid var(--line);
  backdrop-filter: blur(18px);
}
```

## Logo

Use duas versoes:

- logo claro para dark mode;
- logo escuro para light mode.

```css
html[data-theme="dark"] .logo-light { display: none; }
html[data-theme="light"] .logo-dark { display: none; }
```

## Cards De Metricas

Cards devem ser curtos, densos e acionaveis.

Estrutura:

```html
<div class="metric">
  <div class="metric-head">
    <span>Regimes</span>
    <button class="info-btn">i</button>
  </div>
  <strong>3</strong>
</div>
```

Regras:

- Todo card deve ter tooltip/modal de informacao.
- Numeros devem ser formatados.
- Evite texto explicativo direto dentro do card.
- Use barra de progresso apenas quando ela acrescentar leitura visual.

## Paineis

Todo painel analitico deve ter:

- titulo;
- ferramentas no canto direito;
- botao de informacao;
- botao de fullscreen quando fizer sentido;
- corpo sem excesso de padding.

```html
<section class="panel">
  <div class="panel-head">
    <h2>Historical Price by Regime</h2>
    <div class="panel-tools">
      <button class="info-btn">i</button>
      <button class="fullscreen-btn">⛶</button>
    </div>
  </div>
  <div class="chart-wrap"></div>
</section>
```

## Timeline De Regimes

A timeline deve mostrar a sequencia de observacoes modeladas, com cada segmento colorido pelo regime dominante.

Comportamentos esperados:

- clique em segmento seleciona observacao;
- botao `Full view` limpa selecao;
- hover mostra observacao, regime e probabilidade;
- o estado selecionado conversa com inspector, heatmap e grafico principal;
- a sequencia de regimes deve ser estavel e nao mudar de ordem ao clicar.

## Serie Historica

O grafico de linha deve:

- colorir cada trecho pelo regime dominante;
- mostrar datapoints no hover;
- exibir observacao e valor;
- permitir janela de observacoes para series longas;
- ter barra/slider de navegacao horizontal;
- sincronizar o intervalo selecionado com os demais widgets.

Para series longas, nao renderize tudo como uma massa ilegivel. Use janela ativa:

```js
state.rangeStart = 0;
state.rangeEnd = Math.min(total - 1, 120);
```

Todos os widgets devem operar sobre a mesma janela quando o usuario altera esse intervalo.

## Observation Inspector

O inspector mostra a observacao selecionada e deve evitar campos redundantes.

Campos recomendados:

- Observation;
- Price;
- Regime;
- Viterbi Path;
- Dominant Prob.;
- Uncertainty;
- Avg Return;
- Volatility.

Nao use data como campo obrigatorio quando a entrada for apenas uma array numerica. Use indice/observacao como referencia principal.

## Signal Cards

Abaixo da serie historica, use poucos indicadores uteis. Evite indicadores pouco interpretaveis.

Recomendados:

- **Regime Confidence:** probabilidade dominante da observacao selecionada.
- **Vol. Pressure:** volatilidade local normalizada contra o range da janela.

Evite:

- dominance quando replica confidence;
- trend force se nao houver uma definicao intuitiva;
- indicadores que repetem barras ou heatmaps proximos.

## Regime Summary

Tabela compacta por regime:

- Regime;
- Share;
- Avg Prob.;
- Avg Return;
- Vol.

Formatacao:

- retornos e volatilidade em percentual;
- label com swatch colorido;
- ordem de regimes estavel: `bear`, `sideways`, `bull`, depois regimes adicionais.

## Model Selection Criteria

Mostrar AIC, BIC e ICL com nota "lower is better".

Definicoes para tooltip:

- **AIC:** penaliza complexidade de modelo, menor tende a ser melhor.
- **BIC:** penaliza complexidade com mais forca em amostras maiores.
- **ICL:** favorece modelos com classificacao de regimes mais nítida.
- **Final Log-Likelihood:** qualidade final do ajuste; maior e melhor quando os modelos sao comparaveis.

Se o usuario carregar CSV/XLS, esses valores nao devem zerar. Gere diagnosticos coerentes para o novo modelo ou oculte o painel se a metrica nao estiver disponivel.

## Regime Probability Heatmap

O heatmap de probabilidade deve:

- ocupar o frame sem grandes vazios;
- usar linhas fixas por regime, em ordem estavel;
- usar observacao numerica no eixo;
- permitir scroll horizontal em series longas;
- destacar celula no hover;
- mostrar tooltip com observacao, regime e probabilidade;
- manter a mesma cor-base do regime com opacidade/intensidade representando probabilidade.

Nao troque a ordem das linhas quando o usuario clicar em uma celula.

## Transition Heatmap

O heatmap de transicao deve:

- mostrar matriz de Markov;
- ter labels `S0`, `S1`, etc.;
- destacar quadrante no hover;
- mostrar percentual da transicao;
- permitir zoom via botoes e wheel do mouse;
- ter barra de escala de probabilidade na mesma paleta fria da superficie 3D.

## Superficie 3D

A superficie 3D deve funcionar como widget investigativo:

- drag para rotacionar;
- wheel para zoom;
- double click para reset;
- eixos rotulados;
- valores/ticks nos eixos;
- fundo na cor do tema;
- barra de escala ao lado ou embaixo;
- paleta fria consistente com o transition heatmap.

Use canvas nativo quando quiser manter o projeto sem dependencias frontend.

## Tooltips E Modais

Nao coloque explicacao longa diretamente nos cards. Use botao `i` em tudo que precisar de contexto:

- metric cards;
- timeline;
- observation inspector;
- signal cards;
- summary;
- selection criteria;
- heatmaps;
- superficie 3D;
- tabelas;
- controles de upload/exportacao.

Padrao:

```html
<button
  class="info-btn"
  type="button"
  data-info-title="Regime Confidence"
  data-info-body="Dominant regime probability for the selected observation."
  aria-label="Explain Regime Confidence"
>i</button>
```

Ao clicar, abrir modal centralizado com titulo e corpo curto.

## Fullscreen

Graficos e tabelas importantes devem poder entrar em fullscreen:

- serie historica;
- probability heatmap;
- surface 3D;
- transition heatmap;
- Markov matrix;
- latest probabilities.

Fullscreen deve preservar dark/light mode e permitir sair com `Esc` ou botao de fechar.

## Upload De Dados

O upload deve aceitar CSV/TXT e exportacoes XLS textuais com uma unica coluna numerica.

Comportamento esperado:

- usuario carrega arquivo;
- parser extrai numeros no formato `100.50`;
- modelo recalcula cenarios;
- seletor de K permanece disponivel;
- `Auto`, `2`, `3`, `4`, `5` continuam como opcoes quando houver dados suficientes;
- todos os widgets atualizam com o modelo selecionado;
- range window inicia em um periodo legivel, nao na serie inteira.

## Command Palette

Adicionar uma command palette melhora a sensacao de ferramenta profissional.

Atalhos sugeridos:

- `Ctrl K`: abrir comandos;
- Toggle theme;
- Export CSV;
- Export JSON;
- Jump to transition heatmap;
- Reset selection;
- Full view.

## Estados Interativos

Todo elemento clicavel deve ter feedback:

- hover com borda ou brilho leve;
- foco com outline em `--accent`;
- celula selecionada com stroke;
- segmento selecionado com destaque;
- botao ativo com background `--soft`.

## Regras De Composicao

- Use cards apenas para unidades analiticas reais, nao para decorar secoes.
- Nao coloque card dentro de card.
- Evite espacos vazios extensos; ajuste altura do grafico ou reorganize o grid.
- Alinhe finais de frames em colunas paralelas.
- Prefira tabelas compactas a blocos explicativos.
- Evite textos longos dentro da interface; use tooltips.
- Mantenha os controles sempre proximos do widget que controlam.

## Checklist Para Reutilizar Em Outro App

1. Definir tokens `--bg`, `--panel`, `--panel-2`, `--ink`, `--muted`, `--line`, `--accent`.
2. Criar header sticky com logo, titulo e controles.
3. Usar grid principal com painel analitico grande e stack lateral.
4. Adicionar metric cards com botao `i`.
5. Garantir dark/light mode.
6. Usar paleta de regimes consistente em todos os widgets.
7. Adicionar hover, selecao, fullscreen e tooltips nos graficos.
8. Implementar janela de observacoes para series longas.
9. Sincronizar selecao global entre widgets.
10. Validar responsividade em desktop e mobile.

## Copywriting De Interface

Use labels curtos em ingles se a biblioteca publica for internacional:

- Observations
- Modeled Windows
- Regimes
- Final Log-Likelihood
- Latest Regime
- Regime Timeline
- Historical Price by Regime
- Observation Inspector
- Regime Probability Heatmap
- Interactive 3D Return-Volatility Surface
- Transition Heatmap
- Markov Transition Matrix
- Latest Regime Probabilities

Use tooltips para explicacao, nao subtitulos longos.

## Principio Final

O layout deve comunicar capacidade de engenharia: cada componente precisa ter funcao analitica, estado interativo e uma relacao clara com o modelo. A estetica vem da organizacao, da precisao e da coerencia visual, nao de ornamentacao.
