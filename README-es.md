[![English](https://img.shields.io/badge/English-README-blue)](README.md)

# LeonelSearchAPI

Cliente JavaScript para LeonelSearchAPI. Permite realizar búsquedas web, de imágenes, videos y noticias desde tus proyectos utilizando una API key.

## Instalación

```bash
npm install leonelsearchapi
```

## Uso

```javascript
import { LeonelSearchAPI } from "leonelsearchapi";

const client = new LeonelSearchAPI({
  apiKey: process.env.LEONELSEARCH_API_KEY,
});

const response = await client.search({
  query: "latest space news",
  type: "web",
  limit: 5,
});

console.log(response.results);
```

Guarda tu API key en una variable de entorno en lugar de escribirla directamente en el código.

## Tipos de búsqueda

El método `search()` admite los siguientes tipos:

| Tipo | Descripción |
| --- | --- |
| `web` | Páginas y resultados web |
| `image` | Imágenes |
| `video` | Videos |
| `news` | Noticias |

## Referencia de la API

### `client.search(options)`

Realiza una búsqueda. Recibe un objeto con las siguientes propiedades:

| Parámetro | Tipo | Obligatorio | Predeterminado | Descripción |
| --- | --- | --- | --- | --- |
| `query` | `string` | Sí | — | Consulta de búsqueda |
| `type` | `string` | Sí | — | Tipo de búsqueda: `web`, `image`, `video` o `news` |
| `limit` | `number` | No | `10` | Cantidad de resultados por página (1–50) |
| `page` | `number` | No | `1` | Número de página (desde 1) |

### Métodos específicos

También puedes utilizar un método dedicado para cada tipo de búsqueda:

```javascript
const web = await client.searchWeb("space exploration", 10, 1);
const images = await client.searchImages("natural landscapes", 15, 1);
const videos = await client.searchVideos("cooking tutorials", 5, 1);
const news = await client.searchNews("technology trends", 8, 1);
```

Cada método recibe `query`, `limit` y `page`, en ese orden.

## Estructura de respuesta

Los métodos devuelven una promesa que se resuelve en un objeto con la consulta original y sus resultados. Cada resultado tiene la siguiente estructura:

```javascript
{
  query: "search query",
  results: [
    {
      type: "web",
      url: "https://example.com/article",
      title: "Article title",
      text: "Result description or excerpt",
      score: 0
    }
  ]
}
```

El valor de `type` depende del tipo de búsqueda solicitado.

## Soporte para TypeScript

El paquete incluye definiciones de TypeScript. Puedes importar `SearchParams` y `SearchResult` junto con el cliente:

```typescript
import {
  LeonelSearchAPI,
  type SearchParams,
  type SearchResult,
} from "leonelsearchapi";

const client = new LeonelSearchAPI({
  apiKey: process.env.LEONELSEARCH_API_KEY!,
});

async function search(params: SearchParams): Promise<SearchResult[]> {
  const response = await client.search(params);
  return response.results;
}
```

## Manejo de errores

Las solicitudes pueden fallar si la API key no es válida, se alcanza un límite de solicitudes o los parámetros de la petición son incorrectos. Utiliza `try/catch` para manejar estos casos:

```javascript
try {
  const response = await client.search({
    query: "technology",
    type: "news",
  });

  console.log(response.results);
} catch (error) {
  console.error("Search failed:", error.message);
}
```

## Licencia

MIT © 2026 LeonelDev

## Enlaces

- Sitio oficial: [LeonelSearchAPI](https://leonelsearch.qzz.io)
