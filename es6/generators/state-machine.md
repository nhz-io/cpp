# Generator/Iterator based state machine

## Library
```js
function* id(step = 1) {
    let id = 0

    while(id = id + step) {
        yield id
    }
}

function context(transitions, states) {
    let initial = Infinity
    let K = 0

    function transition(fromState, event, toState) {
        const state = states[toState]

        event = parseInt(event, 10)
        toState = parseInt(toState, 10)
        initial = Math.min(initial, toState)
        K = Math.max(K, event, toState)

        if (state) {
            return [fromState, event, function* ($) {
                yield* state([fromState, $])

                return toState
            }]
        }

        return [fromState, event, function* ($) {
            yield $ === undefined ? [toState] : [toState, $]

            return toState
        }]
    }

    function state(transitions, fromState) {
        const events = transitions[fromState]

        fromState = parseInt(fromState, 10)
        initial = Math.min(initial, fromState)
        K = Math.max(K, fromState)

        return Object.keys(events).map($ => transition(fromState, $, events[$]))
    }

    return [
        Object.keys(transitions)
            .reduce((acc, $) => {
                return [...acc, ...state(transitions, $)]
            }, [])
            .reduce((acc, [fromState, event, transition]) => { 
                acc[fromState * (K + 1) + event] = transition
                return acc
            }, {}),
        initial,
        K + 1,
    ]
}

function init(transitions, states = {}) {
    const [generators, initial, K] = context(transitions, states)

    return function* init(state, it) {
        [state, it] = it ? [state, it] : [initial, state]

        for (const $ of it) {
            const [event, data] = Array.isArray($) ? $ : [$]

            const generator = generators[state * K + event]

            if (generator) {
                state = yield* generator(data)
            } 
        }

        return state
    }
}
```

## Sample machine (turnstile)
```js
const [LOCKED, UNLOCKED, PUSH, COIN] = id()

const turnstile = init({
    [LOCKED]: {
        [PUSH]: LOCKED,
        [COIN]: UNLOCKED,
    },
    [UNLOCKED]: {
        [PUSH]: LOCKED,
        [COIN]: UNLOCKED,
    }
})
```

```js
console.log(
    [...turnstile([
        [PUSH], [PUSH], [COIN], [COIN]
    ])].map(id => ({[UNLOCKED]: 'Unlocked', [LOCKED]: 'Locked'}[id]))
)
```

### Result
```js
[ 'Locked', 'Locked', 'Unlocked', 'Unlocked' ]
undefined
```

## No reentry
```js
const noReentry = init(
    {
        [LOCKED]: {
            [PUSH]: LOCKED,
            [COIN]: UNLOCKED,
        },
        [UNLOCKED]: {
            [PUSH]: LOCKED,
            [COIN]: UNLOCKED,
        }
    },
    {
        * [LOCKED]([_, $]) {
            if (_ !== LOCKED) {
                yield [LOCKED]
            }
        },
        * [UNLOCKED]([_, $]) {
            if (_ !== UNLOCKED) {
                yield [UNLOCKED]
            }
        },
    }
)
```

```js
console.log(
    [...noReentry([
        [PUSH], [PUSH], [COIN], [COIN], [COIN], [PUSH], [PUSH], [PUSH]
    ])].map(id => ({[UNLOCKED]: 'Unlocked', [LOCKED]: 'Locked'}[id]))
)
```

### Result
```js
[ 'Unlocked', 'Locked' ]
undefined
```
