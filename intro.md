# Análise Espacial de Redes

**Fundamentos, modelagem e aplicações com Python, NetworkX e OSMnx**

*Alexandro Gularte Schafer*

---

## Apresentação

Ruas, rodovias, ferrovias, redes de abastecimento, rios e corredores ecológicos têm algo em comum: são **redes espaciais**, isto é, conjuntos de elementos localizados no espaço geográfico e ligados por relações de conectividade. Entender como essas redes se estruturam e como pessoas, bens e informações circulam por elas é essencial para o planejamento urbano e regional, a mobilidade, a logística, a saúde pública e a gestão ambiental.

Este livro apresenta, de forma progressiva e aplicada, os conceitos, os métodos e as ferramentas computacionais para a análise de redes espaciais. A teoria dos grafos fornece a linguagem formal; o OpenStreetMap fornece dados abertos para praticamente qualquer lugar do mundo; e o ecossistema Python, em especial as bibliotecas **NetworkX**, **OSMnx** e **GeoPandas**, fornece os meios para transformar essa teoria e esses dados em análises reprodutíveis.

## Público-alvo e pré-requisitos

O livro destina-se a estudantes de graduação e pós-graduação em Geografia, Engenharia Cartográfica, Engenharias Civil e de Transportes, Arquitetura e Urbanismo, Ciência da Computação e áreas afins, bem como a profissionais de geoprocessamento. Pressupõe-se:

- familiaridade com conceitos básicos de cartografia e sistemas de informação geográfica (coordenadas, projeções, dados vetoriais);
- noções de programação em Python (variáveis, listas, dicionários, funções e laços);
- matemática do ensino médio. Os conceitos de teoria dos grafos são apresentados desde o início.

## Organização do livro

O conteúdo está organizado em quatro partes, que vão da abstração matemática às aplicações.

**Parte I · Fundamentos de Grafos e Estruturas Espaciais.** Define formalmente grafos, dígrafos, multigrafos e grafos ponderados, suas representações computacionais e as noções de caminho, distância e conectividade. Introduz o conceito de *grafo espacial* e o problema do caminho mínimo, com exemplos construídos passo a passo na NetworkX.

**Parte II · Modelagem e Aquisição de Dados de Redes (OpenStreetMap/OSMnx).** Apresenta o modelo de dados do OpenStreetMap e o processo pelo qual a OSMnx converte vias em multidígrafos: filtros por modo de deslocamento, simplificação topológica, projeção cartográfica, conversões entre grafos e GeoDataFrames e tratamento de atributos.

**Parte III · Algoritmos e Métricas de Centralidade Espacial.** Formaliza e aplica estatísticas estruturais, índices de conectividade, medidas de eficiência, centralidades de grau, proximidade e intermediação e métricas de orientação da malha viária, sempre discutindo como a natureza espacial da rede altera sua interpretação.

**Parte IV · Roteamento Avançado e Aplicações Práticas.** Trata do roteamento por distância e por tempo de viagem, do mapeamento de isócronas para análise de acessibilidade e da geocodificação de pontos de interesse e sua integração à rede viária.

```{tableofcontents}
```

## Como usar este livro

Cada capítulo é um *notebook* Jupyter executável. É possível:

1. **ler online**, com todos os resultados já renderizados;
2. **executar no Google Colab**, pelo botão de lançamento (ícone de foguete) no topo de cada página;
3. **executar localmente**, criando o ambiente com `conda env create -f environment.yml` ou `pip install -r requirements.txt`, conforme descrito no `README` do repositório.

Os capítulos das Partes II a IV consultam as APIs públicas do OpenStreetMap e, portanto, exigem conexão com a internet. Como os dados do OSM são editados continuamente, os números obtidos por você podem diferir ligeiramente dos apresentados no texto.

## Convenções

Ao longo do livro, usamos blocos destacados com funções específicas:

:::{admonition} Objetivos de Aprendizagem
:class: important
Aparecem no início de cada capítulo e indicam o que você deverá ser capaz de fazer ao concluí-lo.
:::

:::{note}
Complementos conceituais, esclarecimentos de notação e detalhes de implementação.
:::

:::{tip}
Boas práticas, atalhos e recomendações para análises mais eficientes e reprodutíveis.
:::

:::{warning}
Armadilhas comuns, limitações dos dados ou dos métodos e erros de interpretação frequentes.
:::

Definições matemáticas são numeradas por capítulo (por exemplo, Eq. (1.2)), assim como figuras e tabelas. Cada capítulo termina com uma seção de **Exercícios Práticos e Desafios**, que combina questões conceituais, exercícios computacionais e problemas abertos.

## Licença e contribuições

Este livro é distribuído sob a licença **Creative Commons Atribuição-NãoComercial-CompartilhaIgual (CC BY-NC-SA)**. Correções, sugestões e contribuições são bem-vindas por meio de *issues* e *pull requests* no [repositório do projeto](https://github.com/Alexandrogschafer/spatial_network_analysis).
