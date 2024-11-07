---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.4
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---



# 2. INTRODUÇÃO AO USO DO OSMnx PARA ANÁLISE DE REDES ESPACIAIS

O avanço das tecnologias geoespaciais tem permitido aprofundar o entendimento acerca da infraestrutura e a mobilidade urbana, aspectos de grande relevância no contexto do planejamento e desenvolvimento de cidades inteligentes. Dentre as ferramentas emergentes para análise espacial, o OSMnx se destaca como uma biblioteca versátil e acessível para modelar, visualizar e analisar redes viárias a partir dos dados colaborativos do OpenStreetMap (OSM).

Neste capítulo, abordaremos o passo a passo para configurar e utilizar o OSMnx no Google Colab, explorando suas principais dependências e métodos para acessar dados geoespaciais. O capítulo também introduz o leitor ao processo de geocodificação e consultas ao OSM, utilizados para definir áreas de interesse e extrair dados específicos, como edificações, redes de ruas, hospitais, paradas de ônibus e outros elementos de infraestrutura urbana.

Além disso, examinaremos as funcionalidades de visualização do OSMnx e do método `.explore()`, que possibilitam a criação de mapas interativos para a análise de atributos viários, como contagem de interseções, limites de velocidade, e tipos de vias. Essas ferramentas, aliadas aos recursos de personalização gráfica, ampliam o escopo das análises e permitem uma ampla compreensão de como diferentes componentes da rede viária interagem em um determinado contexto urbano.

Este capítulo serve como um guia introdutório para profissionais e estudantes interessados em explorar dados de redes espaciais, oferecendo as bases para aplicações práticas em estudos de mobilidade, acessibilidade, planejamento urbano, entre outros.


##2.1  **O que é OSMnx**
A biblioteca OSMnx é uma ferramenta de código aberto desenvolvida para facilitar o acesso e a análise de dados geoespaciais do OpenStreetMap (OSM). Com OSMnx, é possível baixar e modelar redes viárias e outras infraestruturas urbanas em qualquer localidade do mundo, usando apenas algumas linhas de código. A biblioteca é construída sobre a NetworkX, uma biblioteca para a criação e manipulação de grafos, e a GeoPandas, que permite trabalhar com dados geoespaciais em Python. Essa integração com a NetworkX possibilita a criação de grafos direcionados que representam redes de ruas, com a vantagem de manipular dados espaciais complexos por meio da estrutura de GeoDataFrames da GeoPandas.

Em termos práticos, a biblioteca OSMnx permite a crizção de representações detalhadas de redes urbanas, bem como a análise de elementos como conectividade, acessibilidade e distribuição espacial de infraestruturas. Por exemplo, com OSMnx é possível baixar a rede viária de uma cidade e, em seguida, calcular o caminho mais curto entre dois pontos, analisar a densidade de interseções ou mesmo visualizar a inclinação média das ruas em áreas com declives, integrando dados de elevação.

**Principais funcionalidades**
A OSMnx oferece diversas funcionalidades para a modelagem e análise de redes espaciais:
- **Obtenção de Redes Viárias e Infraestruturas**: Com um único comando, é possível baixar redes viárias completas, limites administrativos e outros elementos geoespaciais, como edificações e pontos de interesse.
- **Modelagem e Manipulação de Infraestruturas**: A biblioteca permite construir grafos complexos que representam redes urbanas, como ruas, ciclovias e trilhas, com suporte para diferentes tipos de mobilidade (carro, bicicleta, caminhada).
- **Simplificação de Topologia**: Para facilitar a análise, OSMnx simplifica automaticamente a topologia das redes, consolidando interseções complexas e removendo nós redundantes, o que gera uma representação precisa e funcional da infraestrutura urbana.
- **Cálculo de Métricas de Rede**: A biblioteca possibilita o cálculo de métricas importantes em estudos de redes urbanas, como centralidade, conectividade e densidade de interseções, permitindo uma compreensão mais aprofundada sobre a acessibilidade e o fluxo na rede.
- **Exportação de Dados**: Redes modeladas podem ser exportadas em formatos compatíveis com outros softwares SIG e de análise de redes, como GraphML, GeoPackage e XML.

Essas funcionalidades tornam a OSMnx uma ferramenta completa para quem deseja estudar redes espaciais urbanas e conduzir análises detalhadas de infraestrutura, transporte e acessibilidade.

**Quando Usar OSMnx**
O uso do OSMnx é adequado para uma variedade de projetos e estudos que envolvem a análise e modelagem de redes espaciais, incluindo:
- **Planejamento Urbano**: o OSMnx permite obter e analisar dados sobre a conectividade de ruas, a distribuição de pontos de interesse e a acessibilidade de diferentes áreas, auxiliando profissionais da área de planejamento urbano a avaliar a estrutura de bairros e a infraestrutura de transporte.
- **Estudos de Acessibilidade**: A biblioteca facilita o cálculo de distâncias e tempos de viagem entre pontos, sendo útil para análises que visam medir a acessibilidade a serviços, como escolas, hospitais e áreas comerciais.
- **Otimização de Rotas**: Por meio da modelagem de redes e do cálculo de rotas, o OSMnx é uma ferramenta útil para estudos de otimização de trajetos, considerando fatores como distância, tempo de viagem e declividade das ruas.
  
Esses recursos tornam a OSMnx uma escolha prática para análises aprofundadas em estudos que envolvem a infraestrutura urbana e a mobilidade, proporcionando uma visão abrangente da conectividade e da acessibilidade nas redes de transporte.


##2.2 **Introdução ao OpenStreetMap**
O OpenStreetMap (OSM) é uma plataforma colaborativa de mapeamento que oferece dados geográficos de código aberto, abrangendo uma grande quantidade de informações, como redes de ruas, limites administrativos, edifícios, pontos de interesse, entre outros. Criado em 2004, o OSM é continuamente atualizado por uma comunidade global de voluntários que contribuem com dados coletados por GPS, imagens de satélite e levantamentos de campo. Esses dados estão disponíveis para qualquer pessoa, permitindo uma ampla variedade de aplicações, desde navegação e análise de redes até planejamento urbano e desenvolvimento de aplicativos.
Para facilitar o acesso a esses dados, o OpenStreetMap disponibiliza uma API (Application Programming Interface), que permite consultas e extrações de informações geográficas por meio de código. A API do OSM é projetada para que os usuários possam recuperar dados geoespaciais específicos, como redes viárias e pontos de interesse, em áreas definidas por coordenadas geográficas, limites de cidades, ou caixas delimitadoras. A biblioteca OSMnx utiliza essa API para acessar automaticamente os dados do OpenStreetMap, permitindo que o usuário baixe e analise redes e outros elementos de infraestrutura urbana de maneira simplificada.
Com a OSMnx, é possível fazer consultas personalizadas ao OpenStreetMap, simplificando o processo de acesso a dados e permitindo análises espaciais detalhadas.
A descrição completa dos tipos de dados disponíveis no **OpenStreetMap (OSM)** pode ser encontrada diretamente na [Wiki do OpenStreetMap](https://wiki.openstreetmap.org/), que é a principal fonte de documentação para todas as tags e categorias de dados mapeados no OSM. 





### **2.3 Preparação do Ambiente e Instalação**

Para utilizar a biblioteca OSMnx, é necessário preparar o ambiente com algumas dependências básicas, configurando o Google Colab para suportar todas as funcionalidades da biblioteca.

#### **Requisitos**
OSMnx depende de outras bibliotecas de Python para realizar a modelagem e análise de redes espaciais. As principais dependências incluem:
- **NetworkX**: Biblioteca para manipulação e análise de grafos, necessária para construir redes viárias e infraestruturas.
- **GeoPandas**: Permite a manipulação de dados geoespaciais, transformando as redes obtidas em estruturas que podem ser visualizadas e analisadas geograficamente.
- **Matplotlib**: Utilizada para visualização de dados e redes.
- **Shapely**: Fornece operações geométricas básicas, como interseção e união de polígonos.

Utilizaremos ainda a biblioteca `mapclassify` fornece métodos para classificação de dados espaciais, facilitando a criação de intervalos ou classes para mapear dados geográficos com diferentes esquemas de categorização.

#### **Instalação da OSMnx no Google Colab**
Para instalar a OSMnx e a mapclassify no Google Colab, execute o seguinte comando para instalar a biblioteca e suas dependências:


```{code-cell}
# !pip install osmnx
# !pip install mapclassify
```

```{code-cell}
import osmnx as ox
import networkx as nx
import matplotlib.pyplot as plt
import geopandas as gpd
import folium
import mapclassify
```

```{code-cell}
print("OSMnx version:", ox.__version__)
```



## **2.3 Acessando Dados do OpenStreetMap**


A OSMnx permite que usuários localizem áreas específicas por meio de geocodificação e consultas diretas ao OpenStreetMap. A geocodificação é o processo de transformar nomes de lugares ou endereços em coordenadas geográficas (latitude e longitude). Com a OSMnx, é possível utilizar essas coordenadas para definir uma área de interesse e baixar dados relevantes do OpenStreetMap.


Inicialmente, vamos tentar obter o polígono de São Paulo:

```{code-cell}
# Nome do local para consulta
nome_local = "São Paulo, Brasil"

# Consulta e obtenção do polígono do local
area = ox.geocode_to_gdf(nome_local)

# Plotagem da área consultada
area.plot()
```

Neste caso, ao usarmos "São Paulo, Brasil", o OSMnx interpretou essa consulta como o município de São Paulo. Assim, ele retornou o polígono correspondente apenas à cidade de São Paulo, em vez de incluir o estado inteiro. Se quisermos obter o polígono referente ao estado, precisamos ser mais específicos na consulta.
Vamos alterar o valor para "Estado de São Paulo, Brasil", pois isso indica ao OSMnx que queremos o contorno do estado:

```{code-cell}
# Nome do local para consulta, especificando o estado
nome_local = "Estado de São Paulo, Brasil"

# Consulta e obtenção do polígono do estado
area = ox.geocode_to_gdf(nome_local)

# Plotagem da área consultada
fig, ax = plt.subplots(figsize=(10, 10))
area.plot(ax=ax, color="lightblue", edgecolor="blue")
ax.set_title("Estado de São Paulo")
plt.show()
```

Com essa mudança, o OSMnx retorna o polígono correspondente ao estado inteiro, não apenas à cidade. Lembre-se, portanto, de que a especificidade é importante: ao referir-se a uma cidade ou um estado, o nome usado na consulta influencia diretamente o resultado obtido.

Agora vamos detalhar como obter o polígono de um bairro específico. Neste capítulo, nossa área de estudo será o bairro da Liberdade, em São Paulo. Para isso, podemos fazer uma consulta específica utilizando o nome do bairro e da cidade.

```{code-cell}
# Nome do local para consulta
bairro = "Liberdade, São Paulo, Brasil"

# Consulta e obtenção do polígono do local
area = ox.geocode_to_gdf(bairro)

# Plotagem da área consultada
area.plot()
```

O código acima busca e plota o polígono do bairro da Liberdade. No entanto, esse plot é estático e não permite interação, sendo útil apenas para uma visualização inicial.

Para uma visualização mais prática e interativa, vamos centralizar o mapa no bairro da Liberdade e adicionar seu contorno em um mapa do Folium. Para fazer isso:

1. **Calcule o centro do polígono**: O centro do bairro será usado como ponto de referência para centralizar o mapa.
2. **Crie um mapa interativo**: Utilizando o Folium, vamos iniciar um mapa com zoom apropriado para visualizar o bairro.
3. **Adicione o polígono ao mapa**: Finalmente, desenhamos o contorno do bairro sobre o mapa interativo.

```{code-cell}
# Consulta e obtenção do polígono do local
area = ox.geocode_to_gdf(bairro)

# Obter o centro do polígono para centralizar o mapa
centro = [area.geometry.centroid.y.values[0], area.geometry.centroid.x.values[0]]

# Criar o mapa usando o centro do bairro
mapa = folium.Map(location=centro, zoom_start=15)

# Adicionar o polígono da área ao mapa
geo_json = area.to_json()
folium.GeoJson(geo_json, name="Bairro da Liberdade").add_to(mapa)

# Exibir o mapa
mapa
```
No código acima:

- **`area.geometry.centroid`**: Calcula o ponto central do polígono para definir a localização inicial do mapa.
- **`folium.Map()`**: Cria um mapa interativo centralizado no ponto calculado e com zoom ajustado.
- **`folium.GeoJson()`**: Transforma o polígono obtido de OSMnx para um formato GeoJSON, adicionando-o ao mapa com o contorno definido.



Agora vamos explorar como extrair e visualizar dados de edificações e infraestrutura urbana no OpenStreetMap (OSM) usando o OSMnx. Vamos nos concentrar em contornos de edifícios e parques no bairro da Liberdade.

### 1. Extraindo e Visualizando Contornos de Edifícios

O OpenStreetMap armazena dados detalhados sobre edifícios em muitas regiões, que incluem os contornos (ou footprints) dos prédios. Podemos acessar essas informações utilizando o OSMnx e a tag `building`.

As **tags do OpenStreetMap (OSM)** são informações associadas aos elementos mapeados que descrevem suas características. Cada elemento geoespacial no OSM – seja um nó, caminho ou relação – é classificado e detalhado usando tags que consistem em pares de chave e valor. Essas tags fornecem contexto e detalham os objetos mapeados, como estradas, edifícios, rios e outros tipos de infraestrutura e pontos de interesse.

Cada tag é um par `chave=valor`. A chave representa o tipo de característica, e o valor fornece a especificação dessa característica. Por exemplo:
- `highway=residential` indica uma estrada de tráfego local.
- `building=yes` descreve um objeto como um edifício.
- `amenity=school` identifica uma escola.

As tags do OSM abrangem diversas categorias, algumas das quais são:

- **Estradas e Trânsito (highway)**: Inclui tipos de vias como `highway=motorway` para rodovias e `highway=footway` para calçadas.
- **Edifícios (building)**: Tag comum usada para classificar tipos de construções. Exemplo: `building=residential`.
- **Áreas Verdes e Natureza (landuse, natural)**: Inclui `landuse=forest` para áreas florestais e `natural=water` para corpos d’água.
- **Serviços e Equipamentos (amenity)**: Para descrever locais de serviço, como `amenity=parking` para estacionamento.
- **Infraestrutura Comercial (shop)**: Classificação de estabelecimentos comerciais, como `shop=supermarket` para supermercados.
- **Recreação e Turismo (leisure, tourism)**: `leisure=park` para parques e `tourism=hotel` para hotéis.

Ao utilizar uma biblioteca como o **OSMnx** para acessar dados do OSM, as tags são essenciais para filtrar e escolher os tipos de elementos que queremos obter. Por exemplo, ao buscar uma rede de estradas, você pode usar a tag `highway` com valores específicos para incluir apenas tipos desejados de vias.

A descrição completa dos tipos de dados disponíveis no **OpenStreetMap (OSM)** pode ser encontrada diretamente na [Wiki do OpenStreetMap](https://wiki.openstreetmap.org/), que é a principal fonte de documentação para todas as tags e categorias de dados mapeados no OSM. Abaixo estão os links diretos para algumas páginas importantes que descrevem os tipos de dados e tags comumente usadas:

- [Página principal das tags](https://wiki.openstreetmap.org/wiki/Tags) – Visão geral das tags do OSM.
- [Highway](https://wiki.openstreetmap.org/wiki/Key:highway) – Descrição das classificações de vias e estradas.
- [Building](https://wiki.openstreetmap.org/wiki/Key:building) – Informações sobre tipos de edifícios.
- [Amenity](https://wiki.openstreetmap.org/wiki/Key:amenity) – Listagem de serviços e equipamentos públicos.
- [Landuse](https://wiki.openstreetmap.org/wiki/Key:landuse) – Categorias de uso da terra e áreas naturais.

Essas páginas fornecem descrições detalhadas para cada tag, oferecendo uma referência fundamental ao usar e interpretar dados do OSM em projetos como visualizações no Folium ou análises de rede com o OSMnx.

Primeiro, vamos definir as configurações e buscar os contornos dos edifícios no bairro.

```{code-cell}
# Definindo a tag para buscar contornos de edifícios
tags = {"building": True}

# Extraindo contornos de edifícios no bairro da Liberdade
gdf_edificios = ox.features_from_place(bairro, tags)
print(gdf_edificios.shape)  # Exibe o número de edifícios encontrados

# Plotando os contornos dos edifícios
fig, ax = ox.plot_footprints(gdf_edificios, figsize=(20, 15))
```

Nesse código:
- **`features_from_place()`**: extrai todos os contornos de edifícios para o bairro especificado.
- **`plot_footprints()`**: plota esses contornos em uma visualização estática para uma visão geral rápida dos edifícios.

#### Visualizando os Contornos dos Edifícios em um Mapa Interativo

Para uma visualização mais detalhada, podemos adicionar esses contornos a um mapa interativo usando o Folium. Isso nos permitirá inspecionar a área e os edifícios com mais flexibilidade.

```{code-cell}
# Criando o mapa centrado no bairro da Liberdade
mapa = folium.Map(location=centro, zoom_start=15)

# Adicionando o polígono da área ao mapa
geo_json = area.to_json()
folium.GeoJson(geo_json, name="Bairro da Liberdade").add_to(mapa)

# Adicionando os contornos dos edifícios ao mapa
for _, row in gdf_edificios.iterrows():
    # Verifica se a geometria é um polígono antes de adicionar ao mapa
    if row.geometry.geom_type == 'Polygon':
        folium.GeoJson(row.geometry.__geo_interface__, style_function=lambda x: {
            'fillColor': 'grey', 'color': 'black', 'weight': 0.5, 'fillOpacity': 0.7
        }).add_to(mapa)

# Exibindo o mapa interativo com edifícios
mapa
```

Aqui, estamos iterando sobre cada edifício e adicionando seu contorno ao mapa. Cada contorno de edifício é preenchido com uma cor cinza para diferenciar as áreas construídas no bairro.


Além de edificações, o OSM também possui dados sobre parques e áreas de lazer. Podemos extrair essas informações utilizando a tag `leisure` com o valor `"park"`.

```{code-cell}
# Definindo a tag para buscar parques
tags = {"leisure": "park"}

# Extraindo parques no bairro da Liberdade
gdf_parques = ox.features_from_place(bairro, tags)
print(gdf_parques.shape)  # Exibe o número de parques encontrados

# Plotando os contornos dos parques
fig, ax = ox.plot_footprints(gdf_parques, figsize=(20, 15))
```

Para uma visualização rápida, o código acima mostra todos os parques em uma visualização estática, similar ao dos edifícios.

Assim como fizemos com os edifícios, podemos adicionar os contornos dos parques em um mapa interativo:

```{code-cell}
# Adicionando os contornos dos parques ao mapa interativo
for _, row in gdf_parques.iterrows():
    if row.geometry.geom_type == 'Polygon':
        folium.GeoJson(row.geometry.__geo_interface__, style_function=lambda x: {
            'fillColor': 'green', 'color': 'darkgreen', 'weight': 0.5, 'fillOpacity': 0.6
        }).add_to(mapa)

# Exibindo o mapa com edifícios e parques sobrepostos
mapa
```

Nesse caso, os parques são representados com preenchimento verde para indicar áreas verdes e diferenciá-los visualmente das edificações.

#### Extraindo Pontos de Ônibus

Para visualizar a infraestrutura de transporte, como pontos de ônibus, podemos utilizar a chave `highway` com o valor `bus_stop`.

```{code-cell}
# Definir a tag para baixar pontos de ônibus
tags = {"highway": "bus_stop"}

# Obter todos os pontos de ônibus na área especificada
gdf_onibus = ox.features_from_place(bairro, tags)

# Exibir o número de pontos de ônibus baixados
print(gdf_onibus.shape)

# Plotar os pontos de ônibus
gdf_onibus.plot()
```

Esse processo é ideal para identificar a cobertura de transporte público em uma área, facilitando análises de acessibilidade urbana.

#### Extraindo Cursos d'Água

Para cursos d'água, que podem ser relevantes em análises ambientais ou de planejamento urbano, utilizamos a chave `waterway`.

```{code-cell}
# Definir a tag para baixar cursos d'água
tags = {"waterway": True}

# Obter todos os cursos d'água na área especificada
gdf_rios = ox.features_from_place(bairro, tags)

# Exibir o número de cursos d'água baixados
print(gdf_rios.shape)

# Plotar os cursos d'água
gdf_rios.plot()
```




#### Amenities

Para facilitar a análise de pontos de interesse em uma área urbana, o OpenStreetMap (OSM) categoriza esses locais específicos sob a chave "amenity", o que abrange uma variedade de estabelecimentos e serviços. Entre eles, encontramos hospitais, escolas, restaurantes, parques e muitos outros que podem ser considerados pontos de interesse ou "amenidades". Com o OSMnx, podemos extrair esses dados e visualizá-los.

Primeiro, vamos fazer uma consulta ampla para obter todos os pontos de interesse em uma área específica, como o bairro da Liberdade. Utilizando `amenity: True`, extraímos todos os pontos disponíveis, sem especificar tipos.

```{code-cell}
# Definir a tag para baixar todas as amenities
tags = {"amenity": True}

# Obter todas as amenities na área especificada
gdf_amenities = ox.features_from_place(bairro, tags)

# Exibir o número de amenities baixadas
print(gdf_amenities.shape)  # Mostra quantas amenidades foram baixadas

# Plotar as amenities em uma visualização estática
gdf_amenities.plot()
```

No código acima:

- **`features_from_place()`**: extrai todas as amenidades no bairro especificado.
- **`.shape`**: exibe o número total de linhas e colunas do GeoDataFrame, indicando quantas amenidades foram encontradas.
- **`.plot()`**: cria uma visualização rápida para verificar a distribuição das amenidades.

### Extraindo Amenidades Específicas

Se quisermos focar em uma amenidade específica, como hospitais, podemos alterar o valor da chave `amenity` para "hospital". Isso ajuda a filtrar apenas os pontos de interesse relevantes.

#### Extraindo Hospitais

```{code-cell}
# Definir a tag para baixar apenas hospitais
tags = {"amenity": "hospital"}

# Obter todos os hospitais na área especificada
gdf_hospitais = ox.features_from_place(bairro, tags)

# Exibir o número de hospitais baixados
print(gdf_hospitais.shape)

# Plotar os hospitais
gdf_hospitais.plot()
```

Esse código restringe a busca para incluir somente hospitais, o que é útil em análises de acesso a serviços de saúde.



É possível visualizar simultaneamente diferentes tipos de amenidades, facilitando a análise da distribuição de edificações, hospitais, paradas de ônibus e parques. Abaixo, detalhamos cada etapa para que você entenda o que está acontecendo em cada linha de código.

### 1. Definindo as Tags para Cada Tipo de Amenidade

Primeiro, definimos as tags que o OSM usa para classificar diferentes tipos de infraestrutura:
- **`tags_edificacoes`**: Identifica edifícios em geral com a tag `"building": True`.
- **`tags_hospitais`**: Define a categoria `"hospital"` para buscar apenas hospitais.
- **`tags_onibus`**: Utiliza a tag `highway` com valor `"bus_stop"` para identificar pontos de ônibus.
- **`tags_parques`**: Usa a tag `"leisure": "park"` para localizar parques.

Essas tags nos permitem buscar cada tipo de infraestrutura separadamente, conforme a classificação no OSM.

```{code-cell}
# Definir as tags para cada tipo de amenidade
tags_edificacoes = {"building": True}
tags_hospitais = {"amenity": "hospital"}
tags_onibus = {"highway": "bus_stop"}
tags_parques = {"leisure": "park"}
```

Usando as tags definidas, baixamos os dados de cada categoria de amenidade para o bairro especificado. Cada consulta retorna um `GeoDataFrame` que armazenará os dados de localização e contorno para cada tipo de amenidade.

```{code-cell}
# Baixar os dados para cada categoria
gdf_edificacoes = ox.features_from_place(bairro, tags_edificacoes)
gdf_hospitais = ox.features_from_place(bairro, tags_hospitais)
gdf_onibus = ox.features_from_place(bairro, tags_onibus)
gdf_parques = ox.features_from_place(bairro, tags_parques)
```


Em seguida, criamos uma figura com `matplotlib` para visualizar todos os dados de amenidades em um único gráfico. O parâmetro `figsize` controla o tamanho do gráfico.

```{code-cell}
# Criar a figura e o eixo para o plot
fig, ax = plt.subplots(figsize=(10, 10))
```

Para cada `GeoDataFrame` baixado, vamos configurar a cor, o tamanho e o rótulo para a legenda. Cada categoria é plotada de forma diferenciada para facilitar a interpretação visual.

```{code-cell}
# Plotar os contornos das edificações
gdf_edificacoes.plot(ax=ax, color="grey", edgecolor="black", alpha=0.5, linewidth=0.5, label="Edificações")

# Plotar os hospitais
gdf_hospitais.plot(ax=ax, color="red", markersize=20, label="Hospitais", alpha=0.7)

# Plotar as paradas de ônibus
gdf_onibus.plot(ax=ax, color="blue", markersize=10, label="Paradas de Ônibus", alpha=0.7)

# Plotar os parques
gdf_parques.plot(ax=ax, color="green", edgecolor="darkgreen", alpha=0.6, linewidth=0.5, label="Parques")

# Adicionar título e legenda
ax.set_title("Edificações, Hospitais, Paradas de Ônibus e Parques - Liberdade", fontsize=15)
ax.legend()

# Remover eixos para uma visualização mais limpa
ax.set_axis_off()

# Exibir o plot
plt.show()
```



## 2.4 Infra-estrutura de Transporte

Para estudar e analisar a infraestrutura de transporte em uma área urbana, uma rede viária é de suma importância. Com o OSMnx, podemos baixar e visualizar a rede de ruas de um bairro específico, como a Liberdade, em São Paulo. A seguir, vamos entender como funciona a representação de uma rede viária e como utilizar o OSMnx para trabalhar com esse tipo de dado.

O OSMnx retorna um **MultiDiGraph** do NetworkX, uma variação do **DiGraph** (grafo direcionado) que permite múltiplas arestas entre o mesmo par de nós. Essa estrutura é adequada para redes viárias, pois permite representar vias com diferentes direções de tráfego, como mão única e mão dupla, com cada direção modelada por uma aresta separada. Em áreas complexas, como rodovias e cruzamentos, onde múltiplos caminhos podem conectar dois pontos, o MultiDiGraph possibilita que cada segmento seja representado individualmente, com atributos específicos como comprimento, limite de velocidade e tipo de via.
O MultiDiGraph no OSMnx possibilita representar corretamente vias com sentidos opostos, rotatórias, rampas e outras estruturas viárias complexas. Cada segmento de via, mesmo que conectado aos mesmos nós, pode conter atributos detalhados (por exemplo, `length`, `maxspeed`, `highway`), garantindo maior precisão e flexibilidade para a análise de redes de transporte urbano.


Em uma rede viária:
- **Nós (Nodes)**: São pontos onde as ruas se encontram, como interseções ou curvas. Eles representam locais de mudança de direção ou de conexão entre vias.
- **Arestas (Edges)**: São os segmentos de ruas que conectam dois nós consecutivos. Cada aresta representa um trecho de rua entre dois pontos.



### Baixando e Visualizando a Rede de Ruas

Para baixar a rede de ruas do bairro da Liberdade para carros, utilizamos o parâmetro `network_type="all"` que busca todas as vias transitáveis por veículos. A função `graph_from_place()` é usada para definir o local e o tipo de rede que queremos baixar.

```{code-cell}
# Baixar a rede de ruas para carros na Liberdade
rede_viaria_carros = ox.graph_from_place(bairro, network_type="all")

# Visualizar a rede de ruas
fig, ax = ox.plot_graph(rede_viaria_carros, bgcolor="w", node_size=0, edge_color="black", edge_linewidth=0.5)
```

No código acima:

- **`graph_from_place()`**: Realiza a consulta e baixa a rede de ruas para o bairro especificado, utilizando o tipo de rede "all" para incluir todas as ruas acessíveis por veículos.
- **`plot_graph()`**: Plota o grafo da rede viária com as seguintes configurações:
  - `bgcolor="w"`: Define o fundo branco.
  - `node_size=0`: Define o tamanho dos nós como zero para ocultá-los no gráfico.
  - `edge_color="black"` e `edge_linewidth=0.5`: Define a cor das ruas como preta e a espessura das linhas como 0.5.



### Tipos de Rede no OSMnx

O OSMnx oferece uma gama de opções para o parâmetro `network_type`, o que permite a personalização dos dados de rede viária conforme o tipo de análise e o modo de transporte. Abaixo, detalhamos cada tipo para ajudar a escolher o mais adequado para diferentes estudos urbanos e de mobilidade:


1. **`"all"`**: Inclui todos os tipos de vias, como ruas, estradas, ciclovias, calçadas, trilhas e caminhos. 

2. **`"all_private"`**: Similar ao tipo `"all"`, mas inclui vias de uso privado, como estradas internas de condomínios e áreas restritas.

3. **`"drive"`**: Inclui somente as vias onde veículos motorizados são permitidos, excluindo ciclovias e calçadas. 

4. **`"drive_service"`**: Expande o tipo `"drive"` para incluir vias de serviço, como acessos a estacionamentos e beco sem saída. 

5. **`"walk"`**: Foca em vias acessíveis para pedestres, incluindo calçadas, trilhas e áreas de tráfego exclusivo para pedestres. 

6. **`"bike"`**: Inclui vias onde é permitida a circulação de bicicletas, como ciclovias e rotas compartilhadas. 

7. **`"drive+bike"`**: Combina vias para veículos e bicicletas, baixando apenas as rotas que permitem o compartilhamento entre ambos. 

8. **`"walk+bike"`**: Inclui vias acessíveis tanto para pedestres quanto para ciclistas.

9. **`"bike+drive+walk"`**: Baixa todas as vias onde veículos, bicicletas e pedestres podem circular, mas exclui vias exclusivas de pedestres e ciclovias exclusivas. Esse tipo permite uma visão completa das rotas urbanas que suportam transporte multimodal.




Vamos baixar e visualizar redes viárias específicas para automóveis e pedestres para o bairro da Liberdade, em São Paulo. 

Primeiro, usamos o `network_type` para filtrar as vias, baixando separadamente as redes viárias para automóveis e pedestres.

A rede viária para automóveis considera apenas as vias onde veículos motorizados podem circular. Isso exclui ciclovias e calçadas.

```{code-cell}
# Baixar a rede de ruas para automóveis na Liberdade
grafo_automoveis = ox.graph_from_place(bairro, network_type="drive")

# Visualizar a rede de ruas para automóveis
fig, ax = ox.plot_graph(grafo_automoveis, bgcolor="w", node_size=0, edge_color="black", edge_linewidth=0.5)
```

A rede viária para pedestres inclui ruas, calçadas e trilhas onde o tráfego de pedestres é permitido.

```{code-cell}
# Baixar a rede de ruas para pedestres na Liberdade
grafo_pedestres = ox.graph_from_place(bairro, network_type="walk")

# Visualizar a rede de ruas para pedestres
fig, ax = ox.plot_graph(grafo_pedestres, bgcolor="w", node_size=0, edge_color="black", edge_linewidth=0.5)
```

Para facilitar a comparação, vamos plotar ambas as redes no mesmo gráfico, usando cores diferentes para distinguir as vias para automóveis e pedestres.

```{code-cell}
# Criar a figura e o eixo para o plot
fig, ax = plt.subplots(figsize=(10, 10))

# Plotar a rede de ruas para pedestres em azul
ox.plot_graph(grafo_pedestres, ax=ax, node_size=0, edge_color="blue", edge_linewidth=0.5, show=False, close=False)

# Plotar a rede de ruas para automóveis em vermelho
ox.plot_graph(grafo_automoveis, ax=ax, node_size=0, edge_color="red", edge_linewidth=0.5, show=False, close=False)

# Adicionar título e legenda manual
ax.set_title("Redes Viárias para Automóveis e Pedestres - Liberdade", fontsize=15)
legend_elements = [plt.Line2D([0], [0], color="red", lw=2, label="Automóveis"),
                   plt.Line2D([0], [0], color="blue", lw=2, label="Pedestres")]
ax.legend(handles=legend_elements, loc="upper right")

# Exibir o plot
plt.show()
```


Além de buscar redes viárias por um nome de local, também é possível definir um ponto central com coordenadas e um raio de busca. Esse método permite delimitar um espaço específico, o que é útil em análises locais.

Para exemplificar, vamos obter a rede viária para automóveis em um raio de 500 metros a partir de um ponto central no bairro da Liberdade.

```{code-cell}
# Coordenadas aproximadas do ponto central do bairro da Liberdade em São Paulo
liberdade_centro = (-23.561414, -46.633176)  # latitude e longitude
um_km = 500  # raio de 500 metros

# Baixar a rede viária para automóveis dentro do raio de 500 metros ao redor do ponto central da Liberdade
G_liberdade = ox.graph_from_point(liberdade_centro, dist=um_km, network_type="drive")

# Visualizar a rede viária
fig, ax = ox.plot_graph(G_liberdade, node_size=0, edge_color="black", edge_linewidth=0.5, bgcolor="w")
```

No código acima: 
- **`liberdade_centro`** define as coordenadas geográficas do ponto central do bairro.
- **`dist=500`** limita a rede viária a 500 metros ao redor desse ponto.




Agora vamos baixar e visualizar redes viárias específicas para bicicletas e pedestres no bairro da Liberdade, exibindo-as em um único gráfico para facilitar a análise comparativa.

Ja havíamos baixado a rede de vias para pedestres. Vamos baixar a rede de vias onde bicicletas podem circular, usando `network_type="bike"`. Isso inclui ciclovias e rotas compartilhadas onde o tráfego de bicicletas é permitido.

```{code-cell}
# Baixar a rede de ruas para bicicletas na Liberdade
grafo_bike = ox.graph_from_place(bairro, network_type="bike")

# Visualizar a rede de ruas para bicicletas
fig, ax = ox.plot_graph(grafo_bike, bgcolor="w", node_size=0, edge_color="black", edge_linewidth=0.5)
```

Para visualizar a rede viária para pedestres e a rede para bicicletas em um mesmo gráfico, utilizamos cores diferentes para cada tipo de via.

```{code-cell}
# Criar a figura e o eixo para o plot
fig, ax = plt.subplots(figsize=(10, 10))

# Plotar a rede de ruas para pedestres em azul
ox.plot_graph(grafo_pedestres, ax=ax, node_size=0, edge_color="blue", edge_linewidth=0.5, show=False, close=False)

# Plotar a rede de ruas para bicicletas em vermelho
ox.plot_graph(grafo_bike, ax=ax, node_size=0, edge_color="red", edge_linewidth=0.5, show=False, close=False)

# Adicionar título e legenda manual
ax.set_title("Redes Viárias para Bicicletas e Pedestres - Liberdade", fontsize=15)
legend_elements = [plt.Line2D([0], [0], color="red", lw=2, label="Bicicleta"),
                   plt.Line2D([0], [0], color="blue", lw=2, label="Pedestres")]
ax.legend(handles=legend_elements, loc="upper right")

# Exibir o plot
plt.show()
```

Para plotar a rede viária com seus nós e arestas:

```{code-cell}
# Visualizar a rede de ruas com destaque para nós e arestas
fig, ax = ox.plot_graph(grafo_rede_viaria, bgcolor="w", node_size=10, node_color="red", edge_color="black", edge_linewidth=0.5)
```

Podemos contar os nós e as arestas do grafo para ter uma visão quantitativa da rede viária:

```{code-cell}
# Contar o número total de nós e arestas no grafo
num_nos = grafo_rede_viaria.number_of_nodes()
num_arestas = grafo_rede_viaria.number_of_edges()

print("Número total de nós:", num_nos)
print("Número total de arestas:", num_arestas)
```

### 2. Visualização Interativa com GeoPandas e Folium

A função `.explore()` do GeoPandas permite criar visualizações interativas dos dados geoespaciais, utilizando o Folium para gerar mapas.


Para visualizar interativamente apenas as arestas ou os nós da rede viária, podemos converter o grafo em GeoDataFrames e utilizar `.explore()`:

```{code-cell}
# Converter as arestas para GeoDataFrame e explorar interativamente
ox.graph_to_gdfs(grafo_rede_viaria, nodes=False).explore()

# Converter os nós para GeoDataFrame e explorar interativamente com mapa base diferente
nodes = ox.graph_to_gdfs(grafo_rede_viaria, edges=False)
nodes.explore(tiles="cartodbpositron", marker_kwds={"radius": 3})
```

Também é possível visualizar ambos em um único mapa interativo, onde as arestas são coloridas de azul e os nós de rosa:

```{code-cell}
# Converter o grafo em GeoDataFrames para nós e arestas
nodes, edges = ox.graph_to_gdfs(grafo_rede_viaria)

# Explorar as arestas no mapa com cor azul e adicionar os nós com cor rosa
m = edges.explore(color="skyblue", tiles="cartodbdarkmatter")
nodes.explore(m=m, color="pink", marker_kwds={"radius": 6})
```

Vamos gerar uma visualização das paradas de ônibus e da rede viária completa.

```{code-cell}
# Definir o nome do local e as opções de mapa
place = "Liberdade, São Paulo, Brasil"
tiles = "cartodbdarkmatter"
mk = {"radius": 4}  # Definir o tamanho dos marcadores

# Explorar as paradas de ônibus no bairro da Liberdade
bus_stops = ox.features_from_place(place, tags={"highway": "bus_stop"})
m = bus_stops.explore(tiles=tiles, color="red", tooltip="name", marker_kwds=mk)

# Explorar a rede viária completa do bairro da Liberdade
road_network = ox.features_from_place(place, tags={"highway": True})
m = road_network.explore(m=m, tiles=tiles, color="blue", tooltip="name")

# Exibir o mapa interativo
m
```


Para realizar uma análise específica em uma área ao redor de um ponto, podemos definir coordenadas centrais e um raio, como no exemplo abaixo, que inclui paradas de ônibus, linhas de metrô e estações de trem na Vila Madalena:

```{code-cell}
# Coordenadas aproximadas do centro da Vila Madalena e raio
centro_vila_madalena = (-23.558213, -46.691616)
raio = 1500  # 1,5 km

# Definir opções de mapa
tiles = "cartodbdarkmatter"
mk = {"radius": 6}

# Explorar paradas de ônibus na área
bus = ox.features_from_point(centro_vila_madalena, tags={"highway": "bus_stop"}, dist=raio)
m = bus.explore(tiles=tiles, color="red", tooltip="name", marker_kwds=mk)

# Explorar linhas de metrô
rail = ox.features_from_point(centro_vila_madalena, tags={"railway": True}, dist=raio)
m = rail.explore(m=m, tiles=tiles, color="yellow", tooltip="name")

# Explorar estações de trem
stations = ox.features_from_point(centro_vila_madalena, tags={"railway": "station"}, dist=raio)
stations.explore(m=m, tiles=tiles, color="green", tooltip="name", marker_kwds=mk)

# Exibir o mapa
m
```



## 2.5 Conversão de grafos de redes viárias


Para realizar diferentes tipos de análises com redes viárias, é útil converter o grafo de uma rede viária em outras estruturas, como um grafo não direcionado, um grafo simplificado ou GeoDataFrames para manipulação geoespacial. Abaixo estão as etapas para realizar essas conversões e explorar os dados.

### 2.5.1 Converter para um MultiGraph Não Direcionado

O OSMnx retorna redes viárias como grafos direcionados (`MultiDiGraph`), que incluem a direção das vias. Em alguns casos, pode ser mais apropriado utilizar uma representação não direcionada, especialmente se a direção das ruas não for relevante para a análise.

```{code-cell}
# Converter o grafo para MultiGraph não direcionado
grafo_nao_direcionado = grafo_rede_viaria.to_undirected()

# Visualizar o grafo não direcionado
fig, ax = ox.plot_graph(grafo_nao_direcionado, node_size=7, node_color='yellow', edge_color='gray', edge_linewidth=0.7)
```

### 2.5.2 Converter para um DiGraph Simplificado

Em situações onde múltiplas arestas entre nós não são necessárias, é possível simplificar o grafo para um `DiGraph`, que representa apenas uma conexão entre pares de nós. Isso pode ser útil para análises que requerem um grafo mais enxuto, sem duplicação de arestas.

```{code-cell}
# Converter para DiGraph simplificado
grafo_direcionado = nx.DiGraph(grafo_rede_viaria)

# Visualizar o grafo simplificado
fig, ax = ox.plot_graph(grafo_direcionado, node_size=7, node_color='yellow', edge_color='gray', edge_linewidth=0.7)
```

### 2.5.3 Converter para GeoDataFrames

Para explorar os atributos dos nós e arestas em um formato geoespacial, podemos converter o grafo em GeoDataFrames. Isso permite manipular os dados viários como pontos (nós) e linhas (arestas) e visualizar atributos específicos.

```{code-cell}
# Converter para GeoDataFrames
gdf_nos, gdf_arestas = ox.graph_to_gdfs(grafo_rede_viaria)

# Visualizar os nós e as arestas
gdf_nos.plot()
gdf_arestas.plot()
```

### 2.6. Explorando Atributos dos Nós e Arestas

Após converter para GeoDataFrames, podemos acessar e analisar os atributos disponíveis para cada nó e aresta. Esses atributos são úteis para entender as características de cada ponto de interseção e de cada segmento de rua.

#### 2.6.1 Atributos dos Nós

Os nós representam interseções ou cruzamentos na rede viária e podem conter atributos como:
- `osmid`: ID do OpenStreetMap.
- Coordenadas de localização (`x` e `y`).
- Outros dados contextuais, como elevação, se disponível.

```{code-cell}
# Contar o número de nós
print("Número total de nós:", len(gdf_nos))

# Exibir as colunas (atributos) dos nós
print(gdf_nos.columns)
```

#### Atributos das Arestas

As arestas representam os segmentos de rua entre os nós e possuem diversos atributos, como:
- `name`: Nome da rua, se mapeado no OSM.
- `length`: Comprimento do segmento de rua em metros.
- `maxspeed`: Velocidade máxima permitida, se disponível.
- `highway`: Tipo de via, por exemplo, `residential` ou `primary`.
- `oneway`: Indica se a via é de mão única.

Esses atributos ajudam a analisar as características da rede viária, como a identificação de ruas principais e a análise de acessibilidade e fluxo de tráfego.

```{code-cell}
# Exibir as colunas (atributos) das arestas
print(gdf_arestas.columns)
```

### Resumo

Essas conversões e análises permitem explorar a rede viária de maneiras variadas e adaptadas às necessidades da análise. A conversão para grafos não direcionados, simplificados ou GeoDataFrames facilita a interpretação dos dados e abre possibilidades para manipulação geoespacial, criação de mapas e análise de atributos específicos em redes viárias.



Os atributos dos nós em um grafo OSMnx fornecem informações detalhadas sobre a rede viária, descrevendo características geoespaciais e de infraestrutura de cada ponto de interseção. Abaixo está uma explicação dos principais atributos encontrados nos nós de uma rede viária baixada com o OSMnx:

1. **`street_count`**:
   - Representa o número de ruas que convergem no nó. Em cruzamentos com várias ruas, o valor será mais alto (geralmente 3 ou 4). Em uma extremidade de rua, como uma rua sem saída, o valor pode ser 1. Esse atributo ajuda a identificar a complexidade e a conectividade dos cruzamentos, sendo útil em análises de tráfego e acessibilidade.

2. **`x` e `y`**:
   - São as coordenadas do nó em longitude (`x`) e latitude (`y`). Esses valores indicam a localização geográfica exata do ponto na rede viária e são de suma importância para mapear a posição dos nós no espaço. Eles permitem também calcular distâncias e relacionar o grafo com outros dados geoespaciais.

3. **`highway`**:
   - Indica o tipo de via ou cruzamento, conforme categorizado no OpenStreetMap. Esse atributo aparece em nós que representam interseções ou pontos de interesse na rede viária, como cruzamentos de pedestres ou ciclovias. Ele é útil para diferenciar tipos de vias e interseções, o que facilita análises de conectividade e planejamento urbano.

4. **`ref`**:
   - Usado para armazenar um identificador de referência do nó, geralmente relacionado ao OpenStreetMap. Esse identificador é útil para consultas adicionais ou para acessar informações de referência diretamente no mapa base do OSM. Esse atributo pode facilitar a integração com outros bancos de dados ou mapas.

Esses atributos fornecem uma descrição completa da rede viária, considerando tanto a estrutura geográfica quanto as especificidades de cada interseção e rua. Eles são importantes em análises de acessibilidade, planejamento de tráfego, conectividade urbana e desenvolvimento de estudos de mobilidade, possibilitando uma compreensão detalhada da infraestrutura viária.




# Lista de colunas a serem verificadas em gdf_nos
colunas = ['y', 'x', 'street_count', 'highway', 'ref', 'geometry']

# Verificar o tipo de dado de cada coluna
print("Tipos de dados de cada coluna em gdf_nos:")
for coluna in colunas:
    print(f"{coluna}: {gdf_nos[coluna].dtype}")


# Visualizar os primeiros nós
gdf_nos.head()



# Verificar valores únicos de 'street_count' em gdf_nos
street_count_unicos = gdf_nos['street_count'].dropna().unique()
print("Valores únicos de street_count:", street_count_unicos)

# Verificar valores únicos de 'highway' em gdf_nos
highway_unicos = gdf_nos['highway'].dropna().unique()
print("Valores únicos de highway:", highway_unicos)

# Verificar valores únicos de 'ref' em gdf_nos
ref_unicos = gdf_nos['ref'].dropna().unique()
print("Valores únicos de ref:", ref_unicos)


Esses atributos fornecem informações detalhadas sobre cada ponto de interseção ou elemento da rede viária, permitindo compreender a complexidade e a funcionalidade de cada nó. Abaixo está uma explicação de cada um dos atributos e dos valores que eles podem assumir:

### Significado dos Atributos e Seus Valores Únicos

1. **`street_count`**: Este atributo indica o número de ruas que se conectam em um determinado nó (geralmente um cruzamento ou interseção). Os valores únicos encontrados são `[3, 4, 5, 1, 6, 2]`, cada um com o seguinte significado:
   - **1**: Indica um ponto final ou uma rua sem saída.
   - **2**: Representa uma conexão direta entre duas ruas, como um segmento reto.
   - **3, 4, 5, 6**: São cruzamentos onde múltiplas ruas se encontram, sugerindo uma estrutura mais complexa da rede viária e possivelmente uma área com mais tráfego.

2. **`highway`**: Este atributo define o tipo de infraestrutura viária associada ao nó. Os valores únicos para `highway` são `['traffic_signals', 'priority', 'mini_roundabout', 'crossing']`, indicando:
   - **`traffic_signals`**: Semáforos presentes no nó, normalmente em cruzamentos movimentados.
   - **`priority`**: Indica um ponto onde a prioridade é regulamentada, como cruzamentos com placas de prioridade.
   - **`mini_roundabout`**: Uma pequena rotatória que facilita o fluxo de tráfego em áreas urbanas.
   - **`crossing`**: Área de travessia para pedestres ou ciclistas, muitas vezes em cruzamentos ou próximos a pontos de transporte público.

3. **`ref`**: Este atributo é um identificador de referência para estradas ou rotas, usado principalmente para rodovias e vias principais. Os valores únicos são `['A-230', 'A-259', 'A-257', 'A-253', 'A-252;A-254', 'A-254', 'A-234', 'A-262', 'A-233']`, onde cada código representa uma estrada ou rota específica. Exemplos incluem:
   - **`A-230`, `A-259`**: Identificadores para estradas principais, comumente rodovias ou vias expressas.
   - **`A-252;A-254`**: Indica um ponto de interseção entre duas estradas, as rotas A-252 e A-254, sugerindo uma interligação importante.





Para começar, filtramos os nós que possuem valores específicos de `street_count`, que indicam a quantidade de ruas conectadas em cada nó. Em seguida, visualizamos esses nós em um mapa, colorindo cada valor de `street_count` de maneira distinta para facilitar a análise da complexidade de cada interseção.

```{code-cell}
# Filtrar apenas os nós com as classes de street_count desejadas
classes_street_count = [1, 2, 3, 4, 5, 6]
gdf_street_count_nos = gdf_nos[gdf_nos['street_count'].isin(classes_street_count)]

# Plotar os nós de street_count com cores diferentes para cada classe
gdf_street_count_nos.explore(
    column="street_count",
    legend=True,
    tiles="CartoDB Positron",
    tooltip="street_count",
    cmap="Paired",
    marker_kwds={
        "radius": 6,
        "fill": True,
    },
    style_kwds={
        "stroke": True,
        "color": "black",
        "weight": 0.5
    }
)
```

Para obter uma visão mais completa, podemos sobrepor os nós filtrados de `street_count` em uma camada de arestas (ruas) representadas com cor suave. Isso cria um mapa onde as interseções estão destacadas sobre a rede viária.

```{code-cell}
# Plotar a camada de arestas (gdf_arestas) com uma cor suave como base
mapa = gdf_arestas.explore(
    tiles="CartoDB Positron",
    color="black",
    style_kwds={
        "weight": 1,
        "opacity": 0.4
    }
)

# Adicionar os pontos de street_count ao mapa, com cores diferenciadas para cada classe
gdf_street_count_nos.explore(
    m=mapa,
    column="street_count",
    legend=True,
    tooltip="street_count",
    cmap="Paired",
    marker_kwds={
        "radius": 6,
        "fill": True,
    },
    style_kwds={
        "stroke": True,
        "color": "black",
        "weight": 0.5
    }
)
```

Além disso, podemos criar um gráfico estático que destaca as interseções com diferentes valores de `street_count`, adicionando uma legenda personalizada para representar o número de ruas conectadas em cada nó. Isso permite visualizar melhor as conexões e a complexidade dos cruzamentos.

```{code-cell}
# Filtrar os nós com as classes de street_count desejadas
classes_street_count = [1, 2, 3, 4, 5, 6]
gdf_street_count_nos = gdf_nos[gdf_nos['street_count'].isin(classes_street_count)]

# Definir o mapa de cores e rótulos personalizados
cmap = plt.get_cmap("Paired")
colors = [cmap(i / len(classes_street_count)) for i in range(len(classes_street_count))]
labels = [f"{count} Rua{'s' if count > 1 else ''}" for count in classes_street_count]

# Criar o plot com ajuste de proporção para deixar espaço para a legenda
fig, ax = plt.subplots(figsize=(12, 10))

# Plotar a camada de arestas (rede viária) com uma cor suave
gdf_arestas.plot(ax=ax, color="gray", linewidth=0.5, alpha=0.5, label="Rede Viária")

# Plotar os pontos de street_count com cores diferentes para cada classe
gdf_street_count_nos.plot(
    ax=ax,
    column="street_count",
    cmap=cmap,
    markersize=30,
    legend=False
)

# Criar uma legenda personalizada com pontos coloridos
legend_elements = [
    Line2D([0], [0], marker='o', color='w', markerfacecolor=colors[i], markersize=8, label=labels[i])
    for i in range(len(classes_street_count))
]

# Adicionar a legenda fora do gráfico
ax.legend(handles=legend_elements, title="Contagem de Interseções (street_count)",
          loc="upper left", bbox_to_anchor=(1, 1))

# Configurações finais do gráfico
plt.title("Interseções de Rua por Contagem de Conexões (street_count)")
plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.grid(False)
plt.tight_layout()
plt.show()
```

Focando agora nos nós com `street_count` igual a 1, que representam pontos finais ou ruas sem saída, é possível sobrepor esses pontos em uma camada de ruas com cor vermelha suave. Isso destaca as terminações da rede viária no mapa.

```{code-cell}
# Filtrar apenas os nós com street_count igual a 1
gdf_street_count_1 = gdf_nos[gdf_nos['street_count'] == 1]

# Plotar a camada de arestas (gdf_arestas) com uma cor vermelha suave como base
mapa = gdf_arestas.explore(
    tiles="CartoDB Positron",
    color="red",
    style_kwds={
        "weight": 1,
        "opacity": 0.5
    }
)

# Adicionar os nós com street_count 1 ao mapa
gdf_street_count_1.explore(
    m=mapa,
    color="blue",
    legend=False,
    tooltip="street_count",
    marker_kwds={
        "radius": 6,
        "fill": True,
    },
    style_kwds={
        "stroke": True,
        "color": "black",
        "weight": 0.5
    }
)
```

Também é possível filtrar os nós de acordo com tipos específicos de `highway`, como `traffic_signals` e `crossing`, para identificar áreas com semáforos, travessias de pedestres, entre outros. Esses pontos são visualizados com cores diferentes para cada tipo.

```{code-cell}
# Filtrar apenas os nós com classes de highway desejadas
classes_highway = ['traffic_signals', 'priority', 'mini_roundabout', 'crossing']
gdf_highway_nos = gdf_nos[gdf_nos['highway'].isin(classes_highway)]

# Plotar os nós de highway com cores diferentes
gdf_highway_nos.explore(
    column="highway",
    legend=True,
    tiles="CartoDB Positron",
    tooltip="highway",
    cmap="Set1",
    marker_kwds={
        "radius": 5,
        "fill": True,
    },
    style_kwds={
        "stroke": True,
        "color": "black",
        "weight": 0.5
    }
)
```

Por fim, para uma visualização ainda mais específica, focamos nos nós que possuem o atributo `highway` igual a `traffic_signals`, destacando apenas os pontos onde há semáforos.

```{code-cell}
# Filtrar apenas os nós com highway igual a 'traffic_signals'
gdf_traffic_signals = gdf_nos[gdf_nos['highway'] == 'traffic_signals']

# Plotar os nós de highway usando uma cor diferente para cada classe
gdf_traffic_signals.explore(
    column="highway",
    legend=True,
    tiles="CartoDB Positron",
    tooltip="highway",
    cmap="Set1",
    marker_kwds={
        "radius": 5,
        "fill": True,
    },
    style_kwds={
        "stroke": True,
        "color": "black",
        "weight": 0.5
    }
)
```



Vamos agora explorar os atributos das arestas na rede viária, que representam as características detalhadas de cada segmento de rua ou conexão entre dois pontos (nós). Primeiramente, faremos uma visualização básica das arestas e contaremos o número total de segmentos.

```{code-cell}
# Visualizar as arestas no mapa
gdf_arestas.plot()

# Contar o número total de arestas
len(gdf_arestas)
```

Além disso, podemos verificar as colunas disponíveis no GeoDataFrame `gdf_arestas` para identificar quais atributos estão presentes nas arestas da rede viária.

```{code-cell}
print(gdf_arestas.columns)
```

Cada um desses atributos das arestas em um grafo OSMnx fornece informações detalhadas sobre os segmentos de rua, ajudando a descrever sua funcionalidade e infraestrutura:

- **`name`**: Nome da rua ou estrada, se disponível (por exemplo, "Main Street" ou "Avenida Brasil").
  
- **`maxspeed`**: Velocidade máxima permitida no segmento, geralmente em km/h ou mph, dependendo da região (por exemplo, 50 para 50 km/h).

- **`lanes`**: Número de faixas de circulação no segmento. Esse valor indica a largura da estrada em termos de capacidade de tráfego.

- **`highway`**: Classificação do tipo de via de acordo com o OpenStreetMap, como "residential", "primary", ou "secondary". Esse atributo indica a hierarquia e o uso da rua, sendo útil para modelagem e simulação de tráfego.

- **`bridge`**: Indica se o segmento de rua é uma ponte (`True` para sim; `False` ou valor ausente para não).

- **`width`**: Largura da via, geralmente em metros, caso essa informação esteja disponível. Esse dado pode ser útil para análises de infraestrutura.

- **`junction`**: Tipo de interseção em que a via participa, como rotatória (roundabout) ou outro tipo de junção.

- **`reversed`**: Indica se a direção da via foi invertida em relação à direção original no OpenStreetMap. Esse atributo é especialmente relevante em vias de mão única para garantir o sentido correto.

- **`oneway`**: Especifica se a via é de mão única (`True` para sim; `False` para mão dupla), sendo um atributo importante para modelagem de redes viárias e de rotas de veículos.

- **`access`**: Define restrições de acesso, como `private` (acesso privado), `no` (sem acesso) ou `yes` (acesso permitido).

- **`length`**: Comprimento do segmento de rua, geralmente em metros, calculado pelo OSMnx com base na geometria da linha. Esse atributo é útil para medir distâncias ao longo da rede viária.

- **`osmid`**: Identificador único do OpenStreetMap para o segmento de rua, permitindo referência cruzada com os dados do OpenStreetMap.

- **`geometry`**: Armazena a geometria da aresta como uma linha (`LineString`), representando a forma do segmento de rua. 








Dando continuidade à exploração dos atributos das arestas, vamos agora examinar os tipos de dados presentes em algumas colunas-chave de `gdf_arestas`. Isso nos ajudará a confirmar que cada coluna contém o tipo de dado esperado, garantindo a consistência das informações que caracterizam cada segmento da rede viária.

Primeiro, definimos uma lista chamada `colunas`, que contém os nomes dos principais atributos que descrevem as arestas, como `osmid`, `oneway`, `lanes`, `highway`, `maxspeed`, entre outros. Cada um desses atributos é útil para entender as propriedades e restrições de cada segmento de rua.

```{code-cell}
# Lista de colunas a serem verificadas
colunas = ['osmid', 'oneway', 'lanes', 'highway', 'maxspeed', 'reversed', 'length',
           'geometry', 'name', 'bridge', 'access', 'junction', 'width']
```

Em seguida, para cada coluna da lista `colunas`, verificamos o tipo de dado usando um loop. Esta verificação é importante para assegurar que os valores estão armazenados corretamente, como números inteiros para o número de faixas (`lanes`) ou dados geométricos para a coluna `geometry`.

```{code-cell}
# Verificar o tipo de dado de cada coluna
print("Tipos de dados de cada coluna em gdf_arestas:")
for coluna in colunas:
    print(f"{coluna}: {gdf_arestas[coluna].dtype}")
```

Além disso, visualizamos as primeiras linhas do GeoDataFrame `gdf_arestas` para observar o conteúdo e a estrutura desses atributos. Esse comando exibe uma amostra dos dados iniciais, oferecendo uma visão geral sobre as informações de cada aresta, incluindo nomes de ruas, tipos de via, número de faixas e geometria do segmento.

```{code-cell}
# Visualizar as primeiras arestas
gdf_arestas.head()
```





No contexto de um grafo de rede viária criado com OSMnx, os identificadores `u`, `v`, `key` e `osmid` desempenham papéis importantes na diferenciação das arestas (ou segmentos de rua):

- **`u`**: Representa o identificador do nó de origem da aresta. Em uma rede viária, esse valor indica o ponto inicial de um segmento de rua.

- **`v`**: Refere-se ao identificador do nó de destino da aresta, ou seja, o ponto final do segmento de rua.

- **`key`**: Esse valor é um identificador adicional que distingue múltiplas arestas entre os mesmos nós `u` e `v`. Em grafos direcionados e multigrafos (grafos que permitem múltiplas arestas entre pares de nós), o `key` assegura que cada conexão entre dois pontos seja única, mesmo que haja várias faixas ou direções diferentes entre os mesmos nós. Por exemplo, se houver múltiplas faixas ou direções distintas para uma rua entre dois pontos, cada segmento terá um valor de `key` exclusivo (como 0, 1, 2, etc.), facilitando a distinção entre eles.

- **`osmid`**: Este identificador único, atribuído pelo OpenStreetMap, permite associar cada segmento de rua ou estrada aos dados originais no OSM. Em alguns casos, `osmid` pode conter múltiplos valores, como `[780219126, 38738766]`, indicando que o segmento é composto por mais de um trecho diferente no OpenStreetMap. Isso é útil para rastrear e atualizar informações da rede viária conforme necessário, especialmente quando uma via é representada por vários identificadores OSM devido a divisões ou modificações na base de dados.

Esses identificadores (`u`, `v`, `key`, e `osmid`) são indispensáveis para manter a estrutura precisa da rede e garantir uma diferenciação clara entre os segmentos de rua, especialmente em redes complexas com vias de mão única, múltiplas faixas e direções variadas.  Eles formam a base para realizar análises de roteamento, conectividade e outras operações complexas na rede viária.


Ao inspecionar as primeiras linhas de `gdf_arestas` usando `gdf_arestas.head()`, podemos ver exemplos desses identificadores. Suponha que vemos os valores `u=587993`, `v=459347433`, e `key=0`. Isso indica que:

- A aresta conecta o nó de origem `587993` ao nó de destino `459347433`.
- Como `key=0`, esta é a primeira (e possivelmente única) conexão direta entre esses dois nós. 

Se houver várias arestas entre os mesmos nós `u` e `v`, cada uma terá um valor de `key` diferente para diferenciar os segmentos.

Além disso, o atributo `osmid` pode, em alguns casos, conter múltiplos valores, como `[780219126, 38738766]`. Isso significa que o segmento é composto por dois trechos diferentes no OpenStreetMap, representando um segmento de rua que está associado a múltiplos identificadores OSM, talvez devido a divisões na via ou a atualizações de dados.











Dando continuidade ao passo a passo, após entender a estrutura das arestas e seus atributos, podemos calcular algumas métricas importantes para a análise da rede viária e explorar os valores únicos de atributos que podem aparecer como listas. Essas informações nos ajudam a obter uma visão detalhada sobre a composição e características dos segmentos de rua.

Primeiro, calculamos o comprimento total da rede viária em metros. A coluna `length` do `gdf_arestas` armazena o comprimento de cada segmento de rua, e somando todos esses valores, obtemos o valor total da extensão da rede.

```{code-cell}
# Calcular o comprimento total da rede viária (em metros)
comprimento_total = gdf_arestas["length"].sum()
print("Comprimento total da rede viária (metros):", round(comprimento_total, 2))
```

Esse valor nos dá uma ideia da extensão total da rede viária em nossa área de estudo, o que é útil para análises de alcance, planejamento urbano, e modelagem de transporte.

Em seguida, analisamos os valores únicos de alguns atributos das arestas que podem aparecer como listas, como `lanes` (número de faixas), `highway` (tipo de via), e `maxspeed` (velocidade máxima permitida). Essas listas ocorrem em casos onde múltiplos valores são atribuídos a um mesmo segmento de rua, devido a diferentes condições em partes do segmento ou a mudanças em sua infraestrutura.

Para tratar esses casos, usamos um loop que converte listas em tuplas e depois utiliza `set()` para coletar os valores únicos. Isso facilita a visualização e a interpretação dos diferentes atributos que podem ser aplicados aos segmentos de rua.

```{code-cell}
# Lista de colunas que podem ter valores em formato de lista
colunas_podem_ter_listas = ['lanes', 'highway', 'maxspeed', 'reversed', 'bridge', 'access', 'junction', 'width']

print("\nValores únicos de atributos que são listas (mostrando listas como conjuntos de valores):")
for coluna in colunas_podem_ter_listas:
    # Converter listas em tuplas para usar com set() e coletar valores únicos
    valores_unicos = set(tuple(valor) if isinstance(valor, list) else valor for valor in gdf_arestas[coluna].dropna())
    print(f"{coluna}: {valores_unicos}")
```

Este trecho de código exibe os valores únicos de cada coluna selecionada, incluindo os casos em que múltiplos valores são atribuídos a um único segmento de rua. Por exemplo, uma estrada pode ter diferentes limites de velocidade em trechos distintos ou variações no número de faixas. Analisar esses valores nos permite entender melhor a variação de infraestrutura ao longo da rede e identificar padrões relevantes para planejamento ou intervenções na área urbana.

A análise desses atributos e suas variações é crucial para uma compreensão mais aprofundada da rede viária. Ela permite adaptar simulações de tráfego, prever tempos de viagem, e identificar áreas que podem necessitar de melhorias, como a adição de faixas em vias com alta demanda ou ajustes em limites de velocidade para otimizar o fluxo de veículos.




Aqui está uma explicação detalhada de cada atributo das arestas e seus possíveis valores, que ajudam a caracterizar a infraestrutura e funcionalidade de cada segmento de rua na rede viária:

- **`lanes`**: Representa o número de faixas de tráfego em um segmento de rua.
  - Valores como `'1'`, `'2'`, `'3'`, `'4'`, e `'5'` indicam a quantidade de faixas em uma única direção.
  - Valores como `('2', '3')` e `('4', '3')` indicam variação no número de faixas em diferentes partes do segmento ou direções. Por exemplo, `('2', '3')` poderia representar 2 faixas em uma direção e 3 na outra.

- **`highway`**: Especifica o tipo de via segundo a classificação do OpenStreetMap, indicando sua importância e função.
  - `residential`: Rua residencial, destinada principalmente ao tráfego local.
  - `primary`, `secondary`, `tertiary`: Classificações que representam a importância da estrada, com `primary` sendo mais importante que `secondary` e assim por diante.
  - Sufixos como `_link` (por exemplo, `tertiary_link`) indicam trechos de conexão, como rampas ou curtos trechos de interseções.

- **`maxspeed`**: Define o limite de velocidade em quilômetros por hora (km/h) para o segmento de rua.
  - Valores como `'20'`, `'30'`, `'40'`, `'50'`, `'60'` representam os limites de velocidade.
  - Tuplas como `('40', '50')` e `('30', '50')` sugerem limites de velocidade distintos para diferentes partes do segmento ou direções. Por exemplo, `('40', '50')` pode indicar 40 km/h em uma direção e 50 km/h na outra.

- **`reversed`**: Indica se a direção do segmento foi invertida em relação à direção original no OpenStreetMap.
  - `True`: A direção foi invertida.
  - `False`: A direção original foi mantida.
  - O valor `(False, True)` sugere que a direção pode variar em diferentes seções do mesmo segmento.

- **`bridge`**: Identifica se o segmento de rua é uma ponte e, em alguns casos, especifica o tipo de estrutura.
  - `yes`: O segmento é uma ponte.
  - `viaduct`: Especifica que o segmento é um viaduto.

- **`access`**: Define as restrições de acesso ao segmento de rua.
  - `yes`: Acesso permitido a todos.
  - `no`: Acesso restrito ou proibido.
  - `destination`: Acesso permitido apenas para destinos específicos, como residentes ou visitantes.

- **`junction`**: Indica o tipo de junção ou interseção.
  - `roundabout`: Indica uma rotatória.

- **`width`**: Define a largura do segmento de rua em metros.
  - Valores como `'10'`, `'5.5'`, e `'8'` indicam a largura da via, geralmente em metros e armazenados como strings.





Dando continuidade ao passo a passo, agora vamos explorar as diferentes maneiras de visualizar pontes, viadutos e outras características das arestas usando o recurso de mapeamento interativo do GeoDataFrame com o método `.explore()`. Vamos também aplicar filtros específicos para destacar segmentos de ruas com determinadas características, como o limite de velocidade e o tipo de via.

Primeiramente, extraímos as arestas que representam pontes e viadutos para visualizá-las separadamente:

```{code-cell}
# Filtrar apenas as pontes e viadutos a partir do GeoDataFrame
gdf_pontes_viadutos = gdf_arestas[gdf_arestas['bridge'].isin(['yes', 'viaduct'])]
```

Também é possível extrair essas arestas diretamente do grafo e, em seguida, convertê-las para um GeoDataFrame:

```{code-cell}
# Extrair arestas que são pontes ou viadutos diretamente do grafo
pontes_viadutos_arestas = [
    (u, v, k) for u, v, k, data in grafo_rede_viaria.edges(keys=True, data=True) 
    if data.get("bridge") in ["yes", "viaduct"]
]

# Criar um subgrafo contendo apenas essas arestas
grafo_pontes_viadutos = grafo_rede_viaria.edge_subgraph(pontes_viadutos_arestas)

# Converter o subgrafo em um GeoDataFrame
gdf_pontes_viadutos = ox.utils_graph.graph_to_gdfs(grafo_pontes_viadutos, nodes=False)
```

Para visualizar as pontes e viadutos no mapa, destacamos o tipo de estrutura usando o método `.explore()`:

```{code-cell}
# Plotar usando .explore()
gdf_pontes_viadutos.explore(
    column="bridge",               # Coluna para diferenciar tipos (pontes e viadutos)
    legend=True,                   # Adicionar legenda para os tipos
    tiles="CartoDB Positron",      # Usar um mapa base mais suave
    tooltip=["name", "bridge"],    # Mostrar o nome e o tipo ao passar o mouse
    style_kwds={
        "color": "red",            # Cor da linha em vermelho
        "weight": 5                # Espessura da linha
    }
)
```

Para enriquecer ainda mais a análise, exploramos as arestas com diferentes colorações, com base em características como o comprimento e o tipo de via. Isso permite identificar visualmente padrões e variações na infraestrutura.

```{code-cell}
# Explorar as arestas do grafo interativamente, coloridas por comprimento
edges.explore(tiles="CartoDB Positron", column="length", cmap="plasma")
```

Diferentes paletas de cores, como "viridis" e "plasma", ajudam a destacar características conforme a necessidade de análise:

```{code-cell}
# Explorar as arestas do grafo com a paleta "viridis" para o tipo de via
edges.explore(tiles="cartodbdarkmatter", column="highway", cmap="viridis")
```

Podemos também visualizar as arestas coloridas de acordo com o tipo de via, para facilitar a identificação dos diferentes tipos de estradas na rede:

```{code-cell}
# Explorar as arestas do grafo interativamente, coloridas por tipo de via
edges.explore(tiles="CartoDB Positron", column="highway", cmap="plasma")
```

Para observar apenas as ruas residenciais, baixamos o grafo filtrando este tipo de via e o visualizamos:

```{code-cell}
# Baixar o grafo da rede viária apenas com ruas residenciais
grafo_residencial = ox.graph_from_place(bairro, network_type="all", custom_filter='["highway"="residential"]')

# Visualizar o grafo com ruas residenciais usando a função explore
ox.plot_graph_folium(grafo_residencial, popup_attribute="name", edge_width=2, edge_color="blue")
```

Para focar em vias com limites de velocidade específicos, como `maxspeed` igual a 40 km/h, aplicamos um filtro para selecionar apenas esses segmentos e destacá-los no mapa:

```{code-cell}
# Filtrar apenas as vias com maxspeed igual a '40'
gdf_maxspeed_40 = gdf_arestas[gdf_arestas['maxspeed'].apply(lambda x: '40' in x if isinstance(x, list) else x == '40')]

# Plotar as vias com maxspeed 40 usando explore()
gdf_maxspeed_40.explore(
    tiles="CartoDB Positron",       # Usar um tile mais suave
    color="red",                    # Cor para as vias filtradas
    legend=False,                   # Não exibir legenda, pois estamos mostrando apenas um valor de maxspeed
    tooltip="maxspeed",             # Exibir o valor de maxspeed ao passar o mouse
    style_kwds={
        "weight": 3,                # Espessura das linhas para melhor visualização
        "opacity": 0.8              # Opacidade das linhas
    }
)
```

Além disso, para visualizar vias que possuem um único valor de `maxspeed` (sem listas de valores), aplicamos um filtro adicional e usamos uma paleta de cores para representar diferentes limites de velocidade:

```{code-cell}
# Filtrar apenas as vias com um único valor de maxspeed (não armazenado como lista)
gdf_maxspeed_unico = gdf_arestas[gdf_arestas['maxspeed'].apply(lambda x: isinstance(x, str))]

# Plotar as vias com maxspeed único usando explore()
gdf_maxspeed_unico.explore(
    tiles="CartoDB Positron",       # Usar um tile mais suave
    column="maxspeed",              # Coluna para diferenciar as vias por velocidade
    cmap="plasma",                  # Usar uma paleta de cores para diferenciar os valores de maxspeed
    legend=True,                    # Exibir legenda para os valores de maxspeed
    tooltip="maxspeed",             # Exibir o valor de maxspeed ao passar o mouse
    style_kwds={
        "weight": 3,                # Espessura das linhas para melhor visualização
        "opacity": 0.9              # Opacidade das linhas
    }
)
```

Essas visualizações ajudam a destacar atributos específicos da rede viária, como tipos de infraestrutura (pontes e viadutos), tipos de vias (residenciais, principais, secundárias), e restrições de velocidade. Elas são ferramentas úteis para analisar a conectividade, a segurança e a acessibilidade da infraestrutura urbana, facilitando decisões em planejamento urbano e gestão de tráfego.




No OpenStreetMap (OSM), o atributo `maxspeed` é utilizado para armazenar o limite de velocidade permitido em um segmento de via. A forma como esse valor é armazenado depende da quantidade de velocidades registradas para o segmento:

- **Quando há apenas um limite de velocidade**, o valor é armazenado como um único número (por exemplo, `50`), representando uma velocidade fixa para todo o trecho.
- **Quando há mais de um limite de velocidade**, o valor é armazenado como uma lista (por exemplo, `['30', '50']`), indicando diferentes limites para seções específicas ou direções da via.

Essa diferença na estrutura dos dados pode dificultar consultas e visualizações, pois requer que o código trate esses casos de maneira diferenciada. Ao realizar uma análise ou criar visualizações com base no limite de velocidade, é necessário verificar se o valor é um número único ou uma lista. Caso contrário, algumas vias podem ser omitidas ou processadas incorretamente, comprometendo a precisão dos resultados.

### Desafios Práticos

- **Consultas**: Consultar vias com um limite específico de `maxspeed` torna-se mais complexo, já que é preciso verificar se o valor é um número ou uma lista. Sem essa diferenciação, filtros simples que busquem por valores únicos podem ignorar vias com listas, excluindo segmentos que possuem limites de velocidade múltiplos.
  
- **Visualizações**: Ao gerar mapas ou gráficos baseados em `maxspeed`, é necessário separar vias com listas de velocidades das que têm um único valor. Isso exige uma lógica adicional para tratar cada caso, assegurando que os segmentos com múltiplos limites sejam corretamente representados.

Esse tipo de estrutura mista nos dados de `maxspeed` exige uma abordagem mais cuidadosa, considerando tanto valores únicos quanto listas. Isso garante que as análises e visualizações sejam mais fiéis à complexidade real das regulamentações viárias, evitando omissões e simplificações que podem impactar a interpretação da rede viária.







Neste capítulo, combinamos o uso de funções específicas do OSMnx e do GeoDataFrame do GeoPandas para consultas e visualizações, aproveitando os pontos fortes de cada abordagem. Dependendo do tipo de análise e visualização desejada, pode ser vantajoso escolher uma das ferramentas ou combinar as duas.

### Usando as Funções do OSMnx

O OSMnx oferece várias vantagens para manipulação e análise de redes viárias:

- **Simplicidade**: As funções do OSMnx são integradas e otimizadas para facilitar a filtragem, consulta e visualização de redes viárias, simplificando o processo de análise.
  
- **Especialização para Redes Viárias**: O OSMnx é desenvolvido especificamente para redes viárias, com métodos para análise de nós, arestas e tipos de via. Isso o torna ideal para consultas de rede rápidas e intuitivas.

- **Interatividade Rápida**: A função `.plot_graph()` permite visualizar rapidamente a rede viária, sendo útil para inspeção visual inicial sem a necessidade de converter os dados para um GeoDataFrame.

No entanto, o uso exclusivo do OSMnx também apresenta limitações:

- **Menor Flexibilidade para Consultas Complexas**: As funções do OSMnx são ótimas para consultas de rede viária básicas, mas podem ser limitadas para operações complexas ou personalizações avançadas de geometria.

### Convertendo para GeoDataFrame

A conversão do grafo para um GeoDataFrame proporciona diversas vantagens:

- **Flexibilidade e Poder de Consulta**: Com o GeoDataFrame, é possível realizar consultas avançadas, combinando a flexibilidade do Pandas com as funcionalidades espaciais do GeoPandas para manipulação e análise de dados.

- **Ampla Gama de Opções de Visualização**: O GeoDataFrame permite personalizar as visualizações usando `.explore()` ou `matplotlib`, com controle sobre estilos, cores, legendas e detalhes gráficos, possibilitando visualizações mais detalhadas e atraentes.

- **Integração com Outras Bibliotecas GIS**: Com o GeoDataFrame, você pode integrar outras bibliotecas GIS, como Shapely e Fiona, para operações espaciais avançadas que não estão disponíveis no OSMnx.

Por outro lado, a conversão para GeoDataFrame também tem uma desvantagem:

- **Processo Adicional de Conversão**: Converter o grafo para GeoDataFrame é um passo extra, que pode ser desnecessário se o objetivo é uma análise básica de rede.

### Recomendação

Para consultas rápidas e análises básicas de rede, as funções do OSMnx são uma escolha eficiente e prática. Entretanto, para análises detalhadas, consultas complexas ou visualizações personalizadas, converter os dados para um GeoDataFrame é mais indicado, pois proporciona maior controle sobre a manipulação e visualização dos dados.




