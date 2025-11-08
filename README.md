# Rota Inteligente: Otimização de Entregas com Algoritmos de IA

## 1. Descrição do Problema, Desafio Proposto e Objetivos

### Descrição do Problema
A empresa "Sabor Express", um serviço de delivery de alimentos na região central da cidade, enfrenta sérios desafios durante horários de pico. As entregas são frequentemente atrasadas devido a rotas ineficientes, definidas manualmente com base apenas na experiência dos entregadores. Isso resulta em aumento de custos com combustível e, crucialmente, insatisfação dos clientes, ameaçando a competitividade da empresa.

### Desafio Proposto
Desenvolver uma solução inteligente, utilizando algoritmos de Inteligência Artificial, para otimizar as rotas de entrega. A cidade será modelada como um grafo, onde nós representam locais de entrega e arestas representam ruas com pesos (distância/tempo). O objetivo é encontrar os menores caminhos entre múltiplos pontos de entrega e, em situações de alta demanda, agrupar entregas próximas utilizando algoritmos de clustering.

### Objetivos
*   Reduzir o tempo de entrega e o custo de combustível.
*   Aumentar a satisfação dos clientes.
*   Automatizar e otimizar o planejamento de rotas.
*   Demonstrar a aplicação de algoritmos de busca em grafos (Dijkstra/A*) e aprendizado não supervisionado (K-Means).

## 2. Explicação Detalhada da Abordagem Adotada

A solução proposta divide-se em duas etapas principais:

1.  **Modelagem da Cidade como um Grafo e Busca do Menor Caminho:**
    *   **Representação:** A cidade é modelada como um **grafo** utilizando a biblioteca `networkx` em Python. Cada local de entrega (bairro, restaurante, casa do cliente) é um **nó** do grafo, e as ruas que conectam esses locais são as **arestas**. Cada aresta possui um **peso**, que representa a distância ou o tempo estimado para percorrer aquela rua.
    *   **Algoritmo de Busca:** Para encontrar a rota mais eficiente entre dois pontos, utilizamos o algoritmo de **Dijkstra** (implementado via `networkx.dijkstra_path`). Este algoritmo garante que o caminho encontrado entre um ponto de partida e um ponto de destino terá o menor peso acumulado (menor distância ou tempo).

2.  **Agrupamento de Entregas com K-Means (Clustering):**
    *   **Cenário:** Em momentos de alta demanda, com muitos pedidos distribuídos pela cidade, torna-se ineficiente que cada entregador faça apenas uma entrega por vez.
    *   **Algoritmo:** Para otimizar o trabalho dos entregadores, aplicamos o algoritmo de **K-Means** (da biblioteca `scikit-learn`). O K-Means agrupa automaticamente os pontos de entrega (representados por suas coordenadas geográficas) em um número `K` de clusters (grupos), onde `K` pode ser o número de entregadores disponíveis ou o número ideal de rotas a serem geradas.
    *   **Benefício:** Cada cluster representa uma "zona" de entregas próximas. Um entregador pode então ser designado para um cluster e planejar sua rota para visitar todos os pontos dentro daquele grupo de forma eficiente, minimizando o deslocamento total.

## 3. Algoritmos Utilizados

*   **Dijkstra:** Algoritmo de busca de menor caminho em grafos ponderados, garantindo a rota mais eficiente entre dois pontos. É uma base fundamental para sistemas de GPS e otimização de rotas.
*   **K-Means:** Algoritmo de clustering (aprendizado não supervisionado) que particiona `N` observações em `K` clusters, onde cada observação pertence ao cluster cujo centroide é o mais próximo. Ideal para agrupar pontos de entrega geograficamente próximos.

## 4. Diagrama do Grafo/Modelo Usado na Solução

Aqui está uma representação visual do grafo utilizado para simular a cidade e o agrupamento de entregas.

### Grafo da Cidade
![Grafo da Cidade](outputs/grafo_visualizado.png)
*Diagrama ilustrando os bairros (nós) e as ruas (arestas) com seus respectivos pesos (distância/tempo). O caminho destacado em verde é um exemplo do menor caminho encontrado pelo algoritmo de Dijkstra.*

### Agrupamento de Entregas (K-Means)
![Agrupamento K-Means](outputs/agrupamento_kmeans.png)
*Gráfico mostrando pontos de entrega agrupados em 3 clusters distintos pelo algoritmo K-Means. Os "X" pretos indicam os centroides de cada cluster, e as diferentes cores representam os grupos de entregas próximas.*

## 5. Análise dos Resultados, Eficiência da Solução, Limitações Encontradas e Sugestões de Melhoria

### Análise dos Resultados
*   A aplicação do algoritmo de Dijkstra demonstrou a capacidade de identificar o caminho mais curto/rápido entre quaisquer dois pontos no grafo, o que é crucial para otimizar viagens individuais.
*   O K-Means conseguiu agrupar de forma eficaz os pontos de entrega geograficamente próximos, o que permite a criação de rotas otimizadas para múltiplos entregadores, reduzindo a distância total percorrida e o tempo de entrega.

### Eficiência da Solução
A combinação de grafos, Dijkstra e K-Means oferece uma base robusta para a otimização de rotas.
*   **Dijkstra:** É eficiente para encontrar o menor caminho em grafos, especialmente quando não são excessivamente densos.
*   **K-Means:** É rápido e escalável para agrupar um grande número de pontos de entrega, tornando-o prático para cenários de alta demanda.
Juntos, eles proporcionam uma melhoria significativa em relação ao planejamento manual, resultando em:
    *   Redução de custos operacionais (combustível).
    *   Maior agilidade nas entregas.
    *   Melhora na satisfação do cliente.

### Limitações Encontradas
1.  **Dados Estáticos:** A implementação atual utiliza um grafo estático e coordenadas fixas. Em um cenário real, os dados de tráfego (pesos das arestas) são dinâmicos e mudam constantemente.
2.  **Problema do Caixeiro Viajante (TSP) dentro dos Clusters:** Após o agrupamento, a otimização da rota *dentro* de cada cluster para visitar todos os pontos uma única vez na ordem mais eficiente é um problema do Caixeiro Viajante (TSP), que é NP-Hard (muito difícil de resolver de forma ótima para muitos pontos). Nossa solução atual não implementa uma heurística TSP avançada para esta fase.
3.  **Capacidade dos Entregadores:** Não foram consideradas restrições de capacidade dos entregadores (ex: quantos pedidos podem levar, tempo máximo de rota).
4.  **Modelo Simplificado:** O grafo atual é pequeno e idealizado. Cidades reais são muito mais complexas.

### Sugestões de Melhoria
1.  **Dados em Tempo Real:** Integrar APIs de mapas (Google Maps, OpenStreetMap) para obter dados de tráfego em tempo real e atualizar dinamicamente os pesos das arestas.
2.  **Heurísticas TSP:** Implementar algoritmos heurísticos ou meta-heurísticos (ex: algoritmo genético, simulated annealing, busca tabu) para resolver o problema do Caixeiro Viajante de forma aproximada dentro de cada cluster, otimizando a sequência de entregas.
3.  **Algoritmo A*:** Para grafos muito grandes, substituir Dijkstra por A* para maior eficiência na busca do menor caminho, utilizando uma função heurística (ex: distância euclidiana até o destino).
4.  **Restrições de Negócio:** Adicionar lógica para considerar capacidade dos veículos, janelas de tempo de entrega, e prioridades de pedidos.
5.  **Interface Gráfica:** Desenvolver uma interface gráfica para os entregadores visualizarem suas rotas otimizadas diretamente em um mapa.
6.  **Outros Algoritmos de Clustering:** Explorar outros algoritmos de clustering como DBSCAN para comparar a qualidade dos agrupamentos.
