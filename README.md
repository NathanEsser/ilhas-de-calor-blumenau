# Ilhas de calor em Blumenau

Estudo da temperatura de superfície dos bairros de Blumenau (SC) a partir de imagens de satélite, feito como primeiro caso demonstrativo do **Núcleo Experimental de Dados**, uma proposta de agência de Ciência de Dados que atenderia os outros cursos da FURB.

A ideia do caso é mostrar, com um exemplo concreto, o que estudantes de dados conseguem entregar, já que muitas vezes quem não trabalha com dados não sabe o que pode pedir.

> **Status:** em andamento. Partes 1 a 8 de 14 concluídas e validadas.

## Pergunta

Quais bairros de Blumenau têm a superfície mais quente, quanto a vegetação explica essa diferença e onde o calor coincide com populações e serviços mais vulneráveis?

| Hipótese | O que afirma | Situação |
| --- | --- | --- |
| H1 | A temperatura de superfície varia vários graus entre os bairros no verão | Confirmada |
| H2 | Bairros com mais vegetação têm a superfície mais fria | Confirmada |
| H3 | Parte dos bairros mais quentes concentra população vulnerável, escolas e unidades de saúde | Próxima etapa |

## Resultados até agora

- **Diferença de 6,5 °C** entre o bairro urbano mais quente (Vila Nova) e o mais fresco (Ribeirão Fresco), na mesma manhã de verão.
- A área urbana fica, em média, cerca de **5 °C acima** da mediana do município, que inclui os morros com mata.
- Bairros mais quentes: Vila Nova, Itoupava Norte, Água Verde, Itoupava Seca, Victor Konder e Centro.
- Relação entre vegetação e temperatura: **r = −0,95**. Cada 0,1 a mais no índice de vegetação (NDVI) está associado a cerca de **2 °C a menos** na superfície.

O mapa, o ranking completo e o gráfico de dispersão estão no notebook.

## Dados

Todos os dados são públicos e gratuitos.

| Dado | Fonte | Uso |
| --- | --- | --- |
| Landsat 8 e 9, Coleção 2, Nível 2 | USGS, acessado pelo Google Earth Engine | Temperatura de superfície e NDVI (pixel de 30 m) |
| Limite do município e bairros (2022) | IBGE, pelo pacote [geobr](https://github.com/ipeaGIT/geobr) do Ipea | Recorte da área e médias por bairro |

*Landsat image courtesy of the U.S. Geological Survey.*

## Como foi feito

1. **Imagens:** 64 cenas dos verões (dezembro a março) de 2022/23 a 2025/26, com menos de 60% de nuvem.
2. **Nuvens:** os pixels com nuvem, borda de nuvem ou sombra são apagados, porque nuvem é fria e faria um bairro parecer mais fresco.
3. **Conversão:** os valores do satélite são transformados em °C e em NDVI com as fórmulas oficiais do USGS.
4. **Composição:** para cada pixel, uso a mediana das observações limpas (cerca de 21 por pixel), o que gera um "verão típico".
5. **Por bairro:** calculo a temperatura e o NDVI médios de cada bairro e monto o ranking.
6. **Teste do rio:** retiro os pixels de água das médias (detalhe abaixo).
7. **Relação calor × vegetação:** correlação e regressão linear simples entre os bairros.

Cada etapa tem um critério de validação definido antes de ver o resultado. Por exemplo, a área calculada do município deu 518,7 km², contra 518,6 km² informados pelo IBGE.

## Principais decisões

| Decisão | Por quê |
| --- | --- |
| Só verões, quatro anos seguidos | As diferenças entre bairros aparecem mais no verão, e vários anos diminuem o peso de um verão atípico |
| Landsat 8 e 9 juntos | Dobra as passagens sobre a cidade e aumenta o número de imagens sem nuvem |
| Mediana em vez de média | Sofre menos com dias extremos e nuvens que escapam da máscara |
| Contornos simplificados em 10 m | Evita erro de tamanho no Earth Engine sem mudar o resultado, já que o pixel tem 30 m |
| Retirar a água das médias | O rio é frio e tem NDVI negativo, o que distorce a comparação entre vegetação e área construída |
| Vila Itoupava apresentada à parte | Tem perfil rural; o resultado foi testado com e sem ela e se mantém |

**Teste do rio.** Boa Vista e Vorstadt tinham pouca vegetação, mas estavam entre os bairros mais frescos. No mapa, os dois ficam junto ao Itajaí-Açu. Retirando os pixels de água, eles passaram a seguir o padrão "mais verde, mais frio", e a partir daí todas as análises usam as médias sem água.

## Limitações

- **Superfície não é ar.** O satélite mede a temperatura de telhados, asfalto e árvores, não a do ar que as pessoas sentem.
- **Associação, não causa.** Bairros com menos vegetação também têm mais asfalto e construção.
- **Horário fixo.** O Landsat passa por volta das 10h; o estudo não mostra o calor da tarde nem da noite.
- **Escala de bairro.** As médias suavizam as variações; pixel a pixel a relação seria mais fraca.
- **Resolução térmica.** O sensor térmico mede em 100 m e o dado é entregue em 30 m, então o mapa mostra padrões de quarteirão, não de telhado.
- **Cobertura.** Os bairros do IBGE cobrem só a área urbana, cerca de 40% do município.

## Roteiro do notebook

| Parte | Conteúdo | Situação |
| --- | --- | --- |
| 1 | Ambiente | ✅ |
| 2 | Conexão com o Earth Engine | ✅ |
| 3 | Área de estudo (município e bairros) | ✅ |
| 4 | Seleção das imagens | ✅ |
| 5 | Nuvens e conversão | ✅ |
| 6 | Composição e mapa | ✅ |
| 7 | Médias por bairro, ranking e teste do rio | ✅ |
| 8 | Relação entre temperatura e vegetação | ✅ |
| 9 | Vulnerabilidade (Censo 2022, escolas e unidades de saúde) | Próxima |
| 10 | Índice de pontos críticos | Planejada |
| 11 | Comparação com estações de temperatura do ar | Planejada |
| 12 | Mapa dos dias mais quentes | Planejada |
| 13 | Simulação "e se houvesse +10% de vegetação?" | Planejada |
| 14 | Ficha de uma página e painel para o portfólio | Planejada |

## Estrutura do repositório

```
ilhas-de-calor-blumenau/
├── ilhas_de_calor_blumenau.ipynb   # notebook com todo o código, explicações e resultados
└── README.md                       # este arquivo
```

A pasta `dados/`, com os arquivos gerados, fica no Google Drive e não vai para o GitHub.

## Como rodar

1. Abra o notebook no Google Colab.
2. Registre um projeto gratuito no [Google Earth Engine](https://console.cloud.google.com/earth-engine) para uso não comercial.
3. Na Parte 2, troque o valor de `PROJETO_EE` pelo ID do seu projeto.
4. Rode as células em ordem. Os resultados são salvos em `MyDrive/ilhas-de-calor-blumenau/dados`.

## Ferramentas

Python · Google Earth Engine · geemap · geobr · geopandas · pandas · scipy · matplotlib · Google Colab

## Autor

Nathan Esser, estudante de Ciência de Dados da FURB, Blumenau (SC).
