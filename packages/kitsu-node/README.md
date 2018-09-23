<h1 align=center>Kitsu Node</h1>

<p align=center>
  <a href=https://www.npmjs.com/package/kitsu-node><img alt=npm src=https://flat.badgen.net/npm/v/kitsu-node></a>
  <a href=https://www.npmjs.com/package/kitsu-node><img alt=npm src=https://flat.badgen.net/npm/dt/kitsu-node></a>
  <a href="https://bundlephobia.com/result?p=kitsu-node"><img alt=bundlephobia src=https://flat.badgen.net/bundlephobia/minzip/kitsu-node></a>
  <a href="https://david-dm.org/wopian/kitsu?path=packages/kitsu-node"><img alt=deps src="https://flat.badgen.net/david/dep/wopian/kitsu/packages/kitsu-node"></a>
</p>

<p align=center>
  <a href=https://travis-ci.org/wopian/kitsu><img alt=travis src=https://flat.badgen.net/travis/wopian/kitsu></a>
  <a href=https://ci.appveyor.com/project/wopian/kitsu><img alt=appveyor src=https://flat.badgen.net/appveyor/ci/wopian/kitsu></a>
  <a href="https://packagephobia.now.sh/result?p=kitsu-node"><img alt=packagephobia src=https://flat.badgen.net/packagephobia/install/kitsu-node></a>
  <a href=https://github.com/wopian/kitsu/graphs/contributors><img alt=contributors src=https://flat.badgen.net/github/contributors/wopian/kitsu></a>
</p>

<p align=center>
  <a href=https://codeclimate.com/github/wopian/kitsu/code?sort=test_coverage><img alt=coverage src=https://flat.badgen.net/codeclimate/coverage/wopian/kitsu></a>
  <a href=https://codeclimate.com/github/wopian/kitsu/code?sort=maintainability><img alt=maintainability src=https://flat.badgen.net/codeclimate/maintainability/wopian/kitsu></a>
  <a href=https://codeclimate.com/github/wopian/kitsu/trends/technical_debt><img alt="technical debt"src=https://flat.badgen.net/codeclimate/tech-debt/wopian/kitsu></a>
  <a href=https://codeclimate.com/github/wopian/kitsu/issues><img alt=issues src=https://flat.badgen.net/codeclimate/issues/wopian/kitsu></a>
</p>

<p align=center>
  <a href=https://paypal.me/wopian><img alt="support me on paypal.me"src=https://flat.badgen.net/badge/support%20me%20on/paypal.me/pink></a>
</p>

<p align=center>A simple, lightweight & framework agnostic <a href=http://jsonapi.org>JSON:API</a> client for <a href=https://kitsu.io>Kitsu.io</a> and other APIs</p>

<p align=center><em>Check out the <a href=https://github.com/wopian/kitsu/blob/master/packages/kitsu/MIGRATING.md>Migration Guide</a> for breaking changes in <code>6.x</code></em></p>

#

## Features

-   JSON-API 1.0 compliant
-   Automatically links relationships to data
-   Uses the [Promise] API
-   Configurable timeout handling

## Response Comparison

<details>
<summary>A GET response by a JSON:API server</summary>

```json5
{
  data: {
    id: '1'
    type: 'articles'
    attributes: {
      title: 'JSON API paints my bikeshed'
    }
    relationships: {
      author: {
        data: {
          id: '42'
          type: 'people'
        }
      }
    }
  }
  included: [
    {
      id: '42'
      type: 'people'
      attributes: {
        name: 'John'
      }
    }
  ]
}
```

</details>

<details open>
<summary>A GET response with kitsu:</summary>

```json5
{
  data: {
    id: '1'
    type: 'articles'
    title: 'JSON API paints my bikeshed'
    author: {
      id: '42'
      type: 'people'
      name: 'John'
    }
  }
}
```

</details>

## Install

### Yarn / NPM

```bash
yarn add kitsu-node
npm install kitsu-node
```

```js
import Kitsu from 'kitsu-node'      // ES Modules and Babel
const Kitsu = require('kitsu-node') // CommonJS and Browserify
```

### Packd CDN

```html
<script src='https://bundle.run/kitsu-node@7?name=Kitsu'></script>
```

## Quick Start

```javascript
// Kitsu.io's API
const api = new Kitsu()

// Other JSON:API servers
const api = new Kitsu({
  baseURL: 'https://api.example/2'
})

// Using with async/await
const res = await api.get('anime')

// Using with Promises
api.get('anime')
  .then(res => { ... })
  .catch(err => { ... })

// Fetching resources (get/fetch)
api.fetch('anime')
api.fetch('anime/1')
api.fetch('anime/1/episodes')

// Creating resources (post/create)
api.create('post', {
  content: 'some content'
})

// Updating resources (patch/update)
api.update('post', {
  id: '1',
  content: 'new content'
})

// Deleting resources
api.remove('post', 1)
```

[More Examples]

If you're working with [Kitsu.io]'s API, their [API docs][kitsu.io api docs] lists all available resources with their attributes and relationships

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [Kitsu](#kitsu)
    -   [Parameters](#parameters)
    -   [Examples](#examples)
    -   [plural](#plural)
        -   [Examples](#examples-1)
    -   [headers](#headers)
        -   [Examples](#examples-2)
    -   [get](#get)
        -   [Parameters](#parameters-1)
        -   [Examples](#examples-3)
    -   [patch](#patch)
        -   [Parameters](#parameters-2)
        -   [Examples](#examples-4)
    -   [post](#post)
        -   [Parameters](#parameters-3)
        -   [Examples](#examples-5)
    -   [delete](#delete)
        -   [Parameters](#parameters-4)
        -   [Examples](#examples-6)
    -   [self](#self)
        -   [Parameters](#parameters-5)
        -   [Examples](#examples-7)

### Kitsu

[packages/kitsu/src/index.js:29-289](https://github.com/wopian/kitsu/blob/0cac34dd976c3f875666adcd7d88800d5ef6b4ad/packages/kitsu/src/index.js#L29-L289 "Source code on GitHub")

Creates a new `kitsu` instance

#### Parameters

-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Options (optional, default `{}`)
    -   `options.baseURL` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Set the API endpoint (default `https://kitsu.io/api/edge`)
    -   `options.headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Additional headers to send with requests
    -   `options.camelCaseTypes` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If true, the `type` value will be camelCased, e.g `library-entries` and `library_entries` become `libraryEntries`  (default `true`)
    -   `options.resourceCase` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** `kebab`, `snake` or `none`. If `kebab`, `/libraryEntries` will become `/library-entries`. If `snake`, `/libraryEntries` will become `/library_entries`, If `none`, `/libraryEntries` will be unchanged (default `kebab`)
    -   `options.pluralize` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If `true`, `/user` will become `/users` in the URL request and `type` will be pluralized in post, patch and delete requests - `user` -> `users` (default `true`)
    -   `options.timeout` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Set the request timeout in milliseconds (default `30000`)

#### Examples

_Using with Kitsu.io's API_

```javascript
const api = new Kitsu()
```

_Using another API server_

```javascript
const api = new Kitsu({
  baseURL: 'https://api.example.org/2'
})
```

_Setting headers_

```javascript
const api = new Kitsu({
  headers: {
    'User-Agent': 'MyApp/1.0.0 (github.com/username/repo)',
    Authorization: 'Bearer 1234567890'
  }
})
```

#### plural

[packages/kitsu/src/index.js:51-52](https://github.com/wopian/kitsu/blob/0cac34dd976c3f875666adcd7d88800d5ef6b4ad/packages/kitsu/src/index.js#L51-L52 "Source code on GitHub")

-   **See: <https://www.npmjs.com/package/pluralize> for documentation**
-   **See: Kitsu constructor options for disabling pluralization**

If pluralization is enabled (default, see Kitsu constructor docs) then pluralization rules can be added

##### Examples

_Adding an uncountable pluralization rule_

```javascript
api.plural.plural('paper') //=> 'papers'
api.plural.addUncountableRule('paper')
api.plural.plural('paper') //=> 'paper'
```

#### headers

[packages/kitsu/src/index.js:66-66](https://github.com/wopian/kitsu/blob/0cac34dd976c3f875666adcd7d88800d5ef6b4ad/packages/kitsu/src/index.js#L66-L66 "Source code on GitHub")

Get the current headers or add additional headers

##### Examples

_Get all headers_

```javascript
api.headers
```

_Get a single header's value_

```javascript
api.headers['User-Agent']
```

_Add or update a header's value_

```javascript
api.headers['Authorization'] = 'Bearer 1234567890'
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** All the current headers

#### get

[packages/kitsu/src/index.js:149-168](https://github.com/wopian/kitsu/blob/0cac34dd976c3f875666adcd7d88800d5ef6b4ad/packages/kitsu/src/index.js#L149-L168 "Source code on GitHub")

Fetch resources (alias `fetch`)

##### Parameters

-   `model` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Model to fetch data from
-   `params` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** JSON-API request queries (optional, default `{}`)
    -   `params.page` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** [JSON:API Pagination](http://jsonapi.org/format/#fetching-pagination)
        -   `params.page.limit` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Number of resources to return in request (Max `20` for Kitsu.io except on `libraryEntries` which has a max of `500`)
        -   `params.page.offset` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Number of resources to offset the dataset by
    -   `params.fields` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Return a sparse fieldset with only the included attributes/relationships - [JSON:API Sparse Fieldsets](http://jsonapi.org/format/#fetching-sparse-fieldsets)
    -   `params.filter` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Filter dataset by attribute values - [JSON:API Filtering](http://jsonapi.org/format/#fetching-filtering)
    -   `params.sort` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Sort dataset by one or more comma separated attributes (prepend `-` for descending order) - [JSON:API Sorting](http://jsonapi.org/format/#fetching-sorting)
    -   `params.include` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Include relationship data - [JSON:API Includes](http://jsonapi.org/format/#fetching-includes)
-   `headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Additional headers to send with request (optional, default `{}`)

##### Examples

_Getting a resource with JSON:API parameters_

```javascript
api.get('users', {
  fields: {
    users: 'name,birthday'
  },
  filter: {
    name: 'wopian'
  }
})
```

_Getting a collection of resources with their relationships_

```javascript
api.get('anime', {
  include: 'categories'
})
```

_Getting a single resource by ID (method one)_

```javascript
api.get('anime/2', {
  include: 'categories'
})
```

_Getting a single resource by ID (method two)_

```javascript
api.get('anime', {
  include: 'categories',
  filter: { id: '2' }
})
```

_Getting a resource's relationship data only_

```javascript
api.get('anime/2/categories')
```

_Handling errors (async/await)_

```javascript
try {
  const { data } = await api.get('anime')
} catch (err) {
  // Array of JSON:API errors: http://jsonapi.org/format/#error-objects
  if (err.errors) err.errors.forEach(error => { ... })
  // Error type (Error, TypeError etc.)
  err.name
  // Error message
  err.message
  // Axios request parameters
  err.config
  // Axios response parameters
  err.response
}
```

_Handling errors (Promises)_

```javascript
api.get('anime')
  .then(({ data }) => { ... })
  .catch(err => {
    // Array of JSON:API errors: http://jsonapi.org/format/#error-objects
    if (err.errors) err.errors.forEach(error => { ... })
    // Error type (Error, TypeError etc.)
    err.name
    // Error message
    err.message
    // Axios request parameters
    err.config
    // Axios response parameters
    err.response
  })
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** JSON-parsed response

#### patch

[packages/kitsu/src/index.js:184-198](https://github.com/wopian/kitsu/blob/0cac34dd976c3f875666adcd7d88800d5ef6b4ad/packages/kitsu/src/index.js#L184-L198 "Source code on GitHub")

Update a resource (alias `update`)

##### Parameters

-   `model` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Model to update data in
-   `body` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Data to send in the request
-   `headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Additional headers to send with request (optional, default `{}`)

##### Examples

_Update a post_

```javascript
api.update('posts', {
  id: '12345678',
  content: 'Goodbye World'
})
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** JSON-parsed response

#### post

[packages/kitsu/src/index.js:221-234](https://github.com/wopian/kitsu/blob/0cac34dd976c3f875666adcd7d88800d5ef6b4ad/packages/kitsu/src/index.js#L221-L234 "Source code on GitHub")

Create a new resource (alias `create`)

##### Parameters

-   `model` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Model to create a resource under
-   `body` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Data to send in the request
-   `headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Additional headers to send with request (optional, default `{}`)

##### Examples

_Create a post on a user's profile feed_

```javascript
api.create('posts', {
  content: 'Hello World',
  targetUser: {
    id: '42603',
    type: 'users'
  },
  user: {
    id: '42603',
    type: 'users'
  }
})
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** JSON-parsed response

#### delete

[packages/kitsu/src/index.js:247-259](https://github.com/wopian/kitsu/blob/0cac34dd976c3f875666adcd7d88800d5ef6b4ad/packages/kitsu/src/index.js#L247-L259 "Source code on GitHub")

Remove a resource (alias `remove`)

##### Parameters

-   `model` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Model to remove data from
-   `id` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number))** Resource ID to remove
-   `headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Additional headers to send with request (optional, default `{}`)

##### Examples

_Remove a user's post_

```javascript
api.delete('posts', 123)
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** JSON-parsed response

#### self

[packages/kitsu/src/index.js:281-288](https://github.com/wopian/kitsu/blob/0cac34dd976c3f875666adcd7d88800d5ef6b4ad/packages/kitsu/src/index.js#L281-L288 "Source code on GitHub")

Get the authenticated user's data

**Note** Requires the JSON:API server to support `filter[self]=true`

##### Parameters

-   `params` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** JSON-API request queries (optional, default `{}`)
    -   `params.fields` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Return a sparse fieldset with only the included attributes/relationships - [JSON:API Sparse Fieldsets](http://jsonapi.org/format/#fetching-sparse-fieldsets)
    -   `params.include` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Include relationship data - [JSON:API Includes](http://jsonapi.org/format/#fetching-includes)
-   `headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Additional headers to send with request (optional, default `{}`)

##### Examples

_Get the authenticated user's resource_

```javascript
api.self()
```

_Using JSON:API parameters_

```javascript
api.self({
  fields: {
    users: 'name,birthday'
  }
})
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** JSON-parsed response

## Contributing

See [CONTRIBUTING]

## Releases

See [CHANGELOG]

## License

All code released under [MIT]

[kitsu.io]: https://kitsu.io

[json:api]: http://jsonapi.org

[promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

[more examples]: https://github.com/wopian/kitsu/tree/master/example

[kitsu.io api docs]: https://kitsu.docs.apiary.io

[migration guide]: https://github.com/wopian/kitsu/blob/master/packages/kitsu/MIGRATING.md

[changelog]: https://github.com/wopian/kitsu/blob/master/packages/kitsu/CHANGELOG.md

[contributing]: https://github.com/wopian/kitsu/blob/master/CONTRIBUTING.md

[mit]: https://github.com/wopian/kitsu/blob/master/LICENSE.md