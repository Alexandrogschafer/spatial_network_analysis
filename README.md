# Livro: Análise Espacial de Redes

Este repositório contém os recursos relacionados ao livro **"Análise Espacial de Redes"**, que tem como objetivo apresentar, de forma acessível e prática, os principais conceitos, técnicas e ferramentas para análise de redes espaciais. O conteúdo é voltado para estudantes, profissionais e pesquisadores interessados em aplicar métodos de análise topológica e espacial em dados geográficos, com foco na utilização da linguagem Python.

O livro explora desde os fundamentos dos grafos até aplicações práticas com bibliotecas como `OSMnx`, `NetworkX`, `GeoPandas`, entre outras.

## Conteúdo

Para acessar o e-book online:  
https://alexandrogschafer.github.io/spatial_network_analysis/intro.html

## Como Contribuir

Se você deseja contribuir para este projeto, relatar problemas ou sugerir melhorias, siga estas etapas:

1. Faça um fork deste repositório.
2. Clone o fork para sua máquina local.
3. Crie uma nova branch para suas contribuições.
4. Faça as alterações desejadas com commits descritivos.
5. Envie um Pull Request para este repositório.

Contribuições, correções e sugestões são muito bem-vindas!

## Estrutura do repositório

```
├── _config.yml            # configuração do Jupyter Book (numeração, bibliografia, execução)
├── _toc.yml               # sumário: quatro partes, seis capítulos
├── intro.md               # prefácio
├── parte1/                # Parte I · Fundamentos de Grafos e Estruturas Espaciais
├── parte2/                # Parte II · Modelagem e Aquisição de Dados (OpenStreetMap/OSMnx)
├── parte3/                # Parte III · Algoritmos e Métricas de Centralidade Espacial
├── parte4/                # Parte IV · Roteamento Avançado e Aplicações Práticas
├── images/                # figuras estáticas
├── references.bib         # bibliografia (sphinxcontrib-bibtex)
├── referencias.md         # página de referências
├── requirements.txt       # dependências (pip)
├── environment.yml        # dependências (conda)
└── PLANO_EDITORIAL.md     # diagnóstico, decisões editoriais e roteiro
```

Cada capítulo é um *notebook* Jupyter (`.ipynb`) que pode ser executado localmente ou no Google Colab.

## Como construir o livro localmente

```bash
# com conda
conda env create -f environment.yml
conda activate analise-espacial-redes

# ou com pip
pip install -r requirements.txt

# construção do HTML (os notebooks são executados e armazenados em cache)
jupyter-book build .
```

O resultado fica em `_build/html/index.html`. Os capítulos 2 a 6 consultam as APIs do OpenStreetMap (Overpass e Nominatim) e exigem conexão com a internet.

## Licença

Este livro é distribuído sob a **Licença Creative Commons Atribuição-NãoComercial-CompartilhaIgual (CC BY-NC-SA)**.  
Consulte o arquivo `LICENSE` para obter mais detalhes.

## Contato

Dúvidas, sugestões ou feedback?  
Sinta-se à vontade para abrir uma *issue* neste repositório ou enviar um e-mail para:  
[alexandroschafer@unipampa.edu.br](mailto:alexandroschafer@unipampa.edu.br)

---

Esperamos que este material seja útil no seu aprendizado e desenvolvimento em geoprocessamento e análise de redes espaciais. Agradecemos seu interesse e participação!
