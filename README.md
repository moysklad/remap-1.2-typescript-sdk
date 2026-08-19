# @moysklad/remap-1.2-sdk

Официальный TypeScript SDK для [МойСклад JSON API 1.2](https://dev.moysklad.ru/doc/api/remap/1.2/).

* npm: https://www.npmjs.com/package/@moysklad/remap-1.2-sdk
* GitHub: https://github.com/moysklad/remap-1.2-typescript-sdk
* OpenAPI-спецификация: https://github.com/moysklad/api-remap-1.2-openapi-specification

Код SDK генерируется из OpenAPI-спецификации, поэтому правки нужно вносить в спецификацию, а не в этот репозиторий.

## Требования

* Node.js >= 22

## Установка

```bash
npm install @moysklad/remap-1.2-sdk
```

## Импорт

ES-модули и TypeScript:

```ts
import { Configuration, ProductsApi } from '@moysklad/remap-1.2-sdk';
```

CommonJS:

```js
const { Configuration, ProductsApi } = require('@moysklad/remap-1.2-sdk');
```

## Авторизация

МойСклад поддерживает Bearer-токен и Basic Auth. Учётные данные передаются в `Configuration`.

Токен доступа:

```ts
const configuration = new Configuration({
  accessToken: process.env.MOYSKLAD_TOKEN,
});
```

Логин и пароль:

```ts
const configuration = new Configuration({
  username: process.env.MOYSKLAD_LOGIN,
  password: process.env.MOYSKLAD_PASSWORD,
});
```

По умолчанию запросы идут на `https://api.moysklad.ru/api/remap/1.2`. Другой адрес задаётся через `basePath`.

## Пример запроса

```ts
import { Configuration, ProductsApi } from '@moysklad/remap-1.2-sdk';

const configuration = new Configuration({
  accessToken: process.env.MOYSKLAD_TOKEN,
});

const productsApi = new ProductsApi(configuration);

const products = await productsApi.getProducts({ limit: 10 });

for (const product of products.rows ?? []) {
  console.log(product.id, product.name);
}
```

Ошибки API возбуждают `ResponseError` с исходным `Response` в поле `response`:

```ts
import { ResponseError } from '@moysklad/remap-1.2-sdk';

try {
  await productsApi.getProducts({ limit: 10 });
} catch (error) {
  if (error instanceof ResponseError) {
    console.error(error.response.status, await error.response.text());
  }
  throw error;
}
```

## Сборка из исходников

```bash
npm install
npm run build
```

Сборка создаёт CommonJS-бандл в `dist/`, ES-модули в `dist/esm/` и файлы деклараций `*.d.ts`.

## Лицензия

Apache-2.0, см. [LICENSE](./LICENSE).
