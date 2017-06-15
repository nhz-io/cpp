# Unique ID generator

```js
const timestamp = (function*() {
    let ts = Date.now()

    while(true) {
        const now = Date.now()
        
        ts = now > ts ? now : ts + 1

        yield ts
    }
})()

function* id() {
    while(true) {
        const id = timestamp.next().value * 1e3
        const limit = id + 1e3

        while(id < limit) {
            yield id++
        }
    }
}
```
