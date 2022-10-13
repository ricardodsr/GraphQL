# Exemplo de consulta do GraphQL
A melhor maneira de entender o GraphQL é vendo alguns exemplos de consultas e respostas. Vamos dar uma olhada em três exemplos adaptados do site do projeto GraphQL, graphql.org.

# O primeiro exemplo mostra como um cliente pode construir uma consulta do GraphQL, solicitando à API que retorne campos específicos no formato determinado.

{
  me {
    name
  }
}

# Uma API GraphQL retornaria um resultado como o abaixo no formato JSON:

{
  "me": {
    "name": "Dorothy"
  }
}

# Um cliente também pode transmitir argumentos como parte da consulta do GraphQL, como neste exemplo:

{
  human(id: "1000") {
    name
    location
  }
}

# O resultado:

{
  "data": {
    "human": {
      "name": "Dorothy,
      "location": "Kansas"
    }
  }
}

A partir daí, as coisas ficam mais interessantes. Com o GraphQL, os usuários podem definir fragmentos reutilizáveis e atribuir variáveis.

Imagine que você precisa solicitar uma lista de IDs e depois uma série de registros de cada ID. Com o GraphQL, é possível elaborar uma consulta que extraia todos os dados que você quer em uma única chamada de API. 

# Portanto, esta consulta:

query HeroComparison($first: Int = 3) {
  leftComparison: hero(location: KANSAS) {
    ...comparisonFields
  }
  rightComparison: hero(location: OZ) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}

# Pode produzir o seguinte resultado:

{
  "data": {
    "leftComparison": {
      "name": "Dorothy",
      "friendsConnection": {
        "totalCount": 4,
        "edges": [
          {
            "node": {
              "name": "Aunt Em"
            }
          },
          {
            "node": {
              "name": "Uncle Henry"
            }
          },
          {
            "node": {
              "name": "Toto"
            }
          }
        ]
      }
    },
    "rightComparison": {
      "name": "Wizard",
      "friendsConnection": {
        "totalCount": 3,
        "edges": [
          {
            "node": {
              "name": "Scarecrow"
            }
          },
          {
            "node": {
              "name": "Tin Man"
            }
          },
          {
            "node": {
              "name": "Lion"
            }
          }
        ]
      }
    }
  }
}
