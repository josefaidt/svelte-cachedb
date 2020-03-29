<script>
  import { setContext, onMount, onDestroy } from 'svelte'
  import { writable } from 'svelte/store'
  import merge from 'deepmerge'
  export let contextKey = 'cachedb'
  export let dbName = 'cachedb'
  export let dbKey = 'svelte-cachedb'
  export let version = 1
  export let autoSave = true
  const store = localStorage
  const structure = {
    dbName,
    version,
  }
  let db = writable(structure)
  const setStore = () => store.setItem(key, JSON.stringify($db))
  
  $: if ($db && autoSave) setStore()
  $: setContext(contextKey, {
    db,
  })
  // ALWAYS SET STRUCTURE
  $: if (!$db.dbName || !$db.version) {
    db.update(s => ({ ...s, ...structure }))
  }

  onMount(() => {
    const raw = store.getItem(key)
    if (raw) {
      let parsed
      try {
        parsed = JSON.parse(raw)
      } catch(e) {
        // womp womp
      } finally {
        if (!parsed.version) {
          console.warn('Error parsing DB version')
          setStore()
        } else {
          // handle different versions and structure updates
          db.set(parsed)
        }
      }
    } else {
      // set default store
      setStore()
    }
  })

  onDestroy(() => {
    // write out
    setStore()
  })
</script>

<slot>
  <!-- content goes here -->
</slot>