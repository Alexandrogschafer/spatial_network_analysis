# Plano editorial: *Análise Espacial de Redes*

Este documento registra o diagnóstico do material didático original, as decisões de reestruturação adotadas e o roteiro para as próximas etapas de edição. Ele não faz parte do livro compilado (está em `exclude_patterns` no `_config.yml`).

## 1. Diagnóstico do material original

### 1.1 Inventário

As fontes do livro estavam no ramo `gh-pages`, misturadas aos artefatos de build (`_build/`, `.ipynb_checkpoints/`, `cache/`, áudios e PDFs de atividades). O ramo `main` continha apenas o `README.md`.

| Arquivo original | Situação encontrada | Destino |
|---|---|---|
| `capitulo1.ipynb` / `.md` | Duas versões divergentes (`.md` de 2024, `.ipynb` de 2025) | Reescrito como `parte1/cap01_fundamentos_grafos.ipynb` |
| `capitulo2.ipynb` / `.md` | Idem | Reescrito como `parte2/cap02_osmnx.ipynb` |
| `capitulo3.ipynb` / `.md` | Idem | Reescrito como `parte3/cap03_metricas_centralidade.ipynb` |
| `capitulo4.ipynb` (`.md` só com títulos) | Notas de aula | Migrado para `parte4/cap04_roteamento.ipynb` (normalização leve) |
| `capitulo5.ipynb` (`.md` só com títulos) | Notas de aula | Migrado para `parte4/cap05_isocronas.ipynb` (normalização leve) |
| `capitulo6.ipynb` | Notas de aula | Migrado para `parte4/cap06_geocodificacao_aplicacoes.ipynb` (normalização leve) |
| `intro.md` | Texto padrão do *template* do Jupyter Book | Reescrito como prefácio |
| `references.bib` | Vazio | Preenchido com 19 referências citadas no texto |
| `referencias.md` | Lista manual, sem vínculo com o `.bib` | Substituído pela diretiva `{bibliography}` |
| `requirements.txt` | `jupyter-book`, `matplotlib`, `numpy` (incompleto) | Reescrito com todas as dependências; criado `environment.yml` |

Os notebooks originais **não continham outputs**: não havia saídas corrompidas nem células com erro armazenado. O `_toc.yml` original tinha um erro de indentação (`- file: referencias` fora da lista `chapters`).

### 1.2 Problemas conceituais e técnicos corrigidos nos capítulos 1 a 3

| Capítulo | Problema | Correção |
|---|---|---|
| 1 | Resíduos de texto gerado automaticamente ("Agora seu markdown está formatado…") | Removidos |
| 1 | Custo por população definido como $p(u)+p(v)$ por aresta, apresentado como "soma das populações" | Formalizada a transformação nó→arco $w'(u,v)=p(v)$ e explicado por que ambas levam ao mesmo caminho ótimo |
| 1 | Conversão implícita de multigrafo para grafo simples sem critério | Aviso sobre `nx.Graph(G_multi)` e agregação explícita pela menor distância |
| 2 | Lista de `network_type` com valores inexistentes (`drive+bike`, `walk+bike`, `bike+drive+walk`, `all_private`) | Tabela com os valores válidos da OSMnx 2.x |
| 2 | `explore(edge_color=…, edge_width=…, popup_attribute=…)`: argumentos inválidos para a GeoPandas | Substituídos por `color`, `style_kwds` e `tooltip` |
| 2 | Mapa com paradas de ônibus sobrescrevia a camada anterior (faltava `m=m`) | Encadeamento correto de camadas |
| 2 | Inclusão de feições uma a uma por `iterrows()` (lento) | Uma única camada por `GeoDataFrame` |
| 2 | Contagens fixas no texto (por exemplo, "10.007 edificações"), que mudam com o OSM | Tabelas calculadas no código e aviso sobre variação temporal |
| 2 | `tiles="CartoDB Positron"`: a xyzservices passou a exigir chave de API | Mapa base da Esri (`Esri.WorldGrayCanvas`), sem chave de API; os blocos do OpenStreetMap são bloqueados em ambientes como o Colab |
| 3 | `ox.basic_stats(G, area=GeoSeries)`: a função espera um escalar | `area_m2 = float(... .area.iloc[0])` |
| 3 | Afirmação de que a OSMnx retorna "multigrafos não direcionados" | Correção: retorna `MultiDiGraph` |
| 3 | Proximidade de **entrada** e de **saída** invertidas (`G.reverse()` rotulado como entrada) | Correção com a definição formal e aviso sobre a convenção da NetworkX |
| 3 | Eficiência global interpretada como métrica ponderada | Explicado que `nx.global_efficiency` é topológica; apresentada a eficiência espacial |
| 3 | Assortatividade positiva interpretada sem ressalvas | Discussão da sensibilidade de $r$ à representação em redes espaciais |

## 2. Lacunas conceituais identificadas e tratamento

| Lacuna | Onde foi tratada |
|---|---|
| Definição formal de grafo, grau, lema do aperto de mãos, densidade | Cap. 1, Eqs. (1.1)–(1.6) |
| Representações computacionais (matriz e lista de adjacência) e esparsidade | Cap. 1, seção "Representações computacionais" |
| Caminhos, distância na rede e conectividade forte e fraca | Cap. 1 |
| Algoritmo de Dijkstra (pseudocódigo, relaxação, complexidade) e A* | Cap. 1, Algoritmo 1.1 |
| **Grafos espaciais**: imersão, planaridade, distância geodésica, circuidade, representação primal e dual | Cap. 1, seção "Grafos espaciais" |
| Modelo de dados do OSM (nós, *ways*, relações e *tags*) e licença ODbL | Cap. 2 |
| Regra de simplificação topológica da OSMnx | Cap. 2, Eq. (2.1) e comparação com e sem simplificação |
| Projeção cartográfica de grafos (CRS) | Cap. 2, seção "Projeção cartográfica" |
| Conversões corretas (`ox.convert.to_undirected`, `to_digraph`) e persistência (GraphML, GeoPackage) | Cap. 2 |
| Distinção `street_count` × grau | Cap. 2 e Cap. 3 |
| Tratamento de atributos com listas e textos (`maxspeed`, `highway`) | Cap. 2 |
| Índices de Kansky ($\beta$, $\gamma$, $\alpha$, $\mu$) | Cap. 3 |
| Fórmulas das centralidades (grau, proximidade com correção de Wasserman-Faust, intermediação) e algoritmo de Brandes | Cap. 3 |
| Intermediação de **arestas** | Cap. 3 |
| Correlação entre centralidades (Spearman) e efeito de borda | Cap. 3 |
| Entropia de orientação e índice de ordem $\varphi$ | Cap. 3 |

## 3. Nova organização (implementada em `_toc.yml`)

- **Parte I · Fundamentos de Grafos e Estruturas Espaciais**
  1. Fundamentos de grafos e redes espaciais *(reescrito)*
- **Parte II · Modelagem e Aquisição de Dados de Redes (OpenStreetMap/OSMnx)**
  2. Aquisição e modelagem com OSM e OSMnx *(reescrito)*
- **Parte III · Algoritmos e Métricas de Centralidade Espacial**
  3. Métricas estruturais e centralidade *(reescrito)*
- **Parte IV · Roteamento Avançado e Aplicações Práticas**
  4. Técnicas de roteamento *(normalização leve)*
  5. Isócronas e acessibilidade espacial *(normalização leve)*
  6. Geocodificação e integração à rede *(normalização leve)*

O capítulo de geocodificação foi posicionado na Parte IV porque seu conteúdo aplicado integra geocodificação, projeção de pontos na rede, rotas e isócronas, funcionando como capítulo de síntese.

## 4. Roteiro das próximas etapas

### 4.1 Padronização dos capítulos 4 a 6

Aplicar o mesmo padrão editorial dos capítulos 1 a 3: objetivos de aprendizagem, prosa contínua em lugar de explicações linha a linha do código, formalização em LaTeX, *admonitions*, legendas e rótulos de figuras e exercícios ao final. Pontos específicos:

- **Cap. 4 (roteamento):** formalizar tempo de viagem ($t_e = \ell_e / v_e$), a imputação de velocidades por `ox.add_edge_speeds`, a heurística admissível do A* e os *k* caminhos mais curtos (`ox.k_shortest_paths`); discutir restrições de conversão (relações `restriction` do OSM); unificar os dois títulos "Roteamento considerando velocidade" e corrigir "Calculo" → "Cálculo".
- **Cap. 5 (isócronas):** formalizar a isócrona como $I_T(s) = \{v \in V : d_G(s,v) \leq T\}$; comparar envoltória convexa, *buffer* e envoltória côncava (`shapely.concave_hull`); corrigir a numeração duplicada "5.3.2.2" do original; introduzir métricas de acessibilidade cumulativa e gravitacional.
- **Cap. 6 (geocodificação):** reduzir as tabelas comparativas de APIs a uma nota, pois os preços e as condições mudam com frequência; explicitar a política de uso do Nominatim (1 requisição/s); substituir o CSV embutido em texto por um arquivo em `dados/`.

### 4.2 Capítulos novos sugeridos

- **Parte III:** *Detecção de comunidades e regionalização em redes espaciais* (Louvain/Leiden, modularidade espacial).
- **Parte IV:** *Localização-alocação* (p-medianas, cobertura máxima) e *Redes multimodais e GTFS*.
- **Apêndices:** configuração do ambiente; revisão de Python/GeoPandas; glossário de termos.

### 4.3 Infraestrutura

- **Publicação automática:** criar um *workflow* do GitHub Actions que construa o livro a partir do `main` e publique no GitHub Pages. Isso substituiria o `gh-pages` atual, que mistura fontes e artefatos. Recomenda-se migrar o Pages para "GitHub Actions" como fonte, preservando o ramo `gh-pages` atual como histórico.
- **Execução reprodutível:** como os capítulos 2 a 6 dependem de APIs externas, avaliar salvar os grafos usados no livro em `dados/` (GraphML) com data de extração, carregando-os quando disponíveis.
- **Logo:** o `logo.png` atual é o logotipo padrão do Jupyter Book; substituir por uma identidade visual própria.
