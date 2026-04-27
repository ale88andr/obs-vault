```js
window.domains = [...new Set(performance.getEntriesByType('resource').map(r => (new URL(r.name)).hostname))]; console.log(domains);
```

для активации скрипта

```
allow pasting
```

