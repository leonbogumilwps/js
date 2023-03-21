- Douglas Crockford promotes OOP without classes and prototypes:

```js
// simulate named arguments via object destructuring
function createAccount({ initialBalance, accountId }) {
    let balance = initialBalance || 0;
    const id = accountId || 0;

    //     shorthand for { deposit: deposit, getBalance: getBalance }
    return Object.freeze({ deposit, getBalance });

    function deposit(amount) {
        balance += amount;
    }

    function getBalance() {
        return balance;
    }
}

const account = createAccount({ initialBalance: 1000, accountId: 42 });
// { deposit: [Function: deposit], getBalance: [Function: getBalance] }
account.deposit(234);
account.getBalance() // 1234
account.balance      // undefined

createAccount(1234, 42).getBalance === account.getBalance // false
```

- Encapsulation
- Frozen interface
- Much simpler language without:
  - `this`
  - `new`
  - `prototype`
  - `__proto__`
  - `constructor`
  - `class`
- Increased memory usage
- Looks alien to Java programmers
