# svelte-cachedb

A JSON database leveraging Svelte's writable stores and local storage

## Getting Started

1. Install `svelte-cachedb`: `yarn add -D svelte-cachedb`
2. Add context provider to Svelte

   ```html
   <!-- src/index.svelte -->
   <script>
     import CacheDB from 'svelte-cachedb'
     import App from './App.svelte'
   </script>

   <CacheDB>
     <App />
   </CacheDB>
   ```

3. Access with `getContext('cachedb')`

   ```html
   <script>
     import { getContext } from 'svelte'
     const { db } = getContext('cachedb')

     // updates to db store will automatically write out to localStorage
     db.update((d) => ({ ...d, newProp: 'newValue' }))
   </script>
   ```

## Customization

| Property     | Default          | Description                                                                                  |
| ------------ | ---------------- | -------------------------------------------------------------------------------------------- |
| `contextKey` | `cachedb`        | key used to store in Svelte's context (i.e. `getContext('cachedb')`)                         |
| `dbName`     | `cachedb`        | name used for localStorage database name                                                     |
| `dbKey`      | `svelte-cachedb` | value used to store database in localStorage (i.e. `localStorage.getItem('svelte-cachedb')`) |
| `version`    | 1                | value used for storing database version                                                      |
| `autoSave`   | true             | when the db store updates, autosaves to localStorage                                         |

**In Action**

```html
<script>
  import CacheDB from 'svelte-cachedb'
  import App from './App.svelte'
</script>

<CacheDB
  contextKey="cachedb"
  dbName="cachedb"
  dbKey="svelte-cachedb"
  version={1}
  autoSave={true}
>
  <App />
</CacheDBcontextKey="cachedb">
```
