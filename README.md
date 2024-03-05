# clean-code-javascript

clean-code-javascript

## template literals

**Dirty code**

```js
const name = "Peter";
const message = "Hi" + name + ",";
```

**Clean code**

```js
var name = "Peter";
var message = `Hi ${name},`;
```

## Spread Syntax

**Dirty code**

```js
const pikachu = { name: "Pikachu 🐹" };
const stats = { hp: 40, attack: 60, defense: 45 };

pikachu["hp"] = stats.hp;
pikachu["attack"] = stats.attack;
pikachu["defense"] = stats.defense;

// OR

const lvl0 = Object.assign(pikachu, stats);
const lvl1 = Object.assign(pikachu, { hp: 45 });
```

**Clean code**

```js
const pikachu = { name: "Pikachu 🐹" };
const stats = { hp: 40, attack: 60, defense: 45 };

const lvl0 = { ...pikachu, ...stats };
const lvl1 = { ...pikachu, hp: 45 };
```

**Array spread**

```js
let pokemon = ["Arbok", "Raichu", "Sandshrew"];

// dirty code
pokemon.push("Bulbasaur");
pokemon.push("Metapod");
pokemon.push("Weedle");

// clean code

// Push
pokemon = [...pokemon, "Bulbasaur", "Metapod", "Weedle"];

// Shift

pokemon = ["Bulbasaur", ...pokemon, "Metapod", "Weedle"];
```

## Rest Syntax

**Dirty code**

```js
function totalHitPoints(a, b, c, d) {
    return a + b + c + d;
}
```

**Clean code**

```js
function totalHitPoints(...hits) {
    return hits.reduce((a, b) => a + b);
}

totalHitPoints(1, 2, 3, 4, 5, 6, 7);
```

## Destructuring

**Object**

```js
const user = {
    name: "John",
    age: 30,
};

// dirty code
const name = user.name;
const age = user.age;

// clean code
const { name: userName, age: userAge } = user;
```

**Array**

```js
// dirty code
const numbers = [1, 2, 3, 4, 5];

const first = numbers[0];
const second = numbers[1];
const rest = numbers.slice(2);

console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5]

// clean code
const numbers = [1, 2, 3, 4, 5];

const [first, second, ...rest] = numbers;

console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5]
```

### Function params

```js
const User = {
    name: "John",
    type: "admin",
    order: 10,
};

// dirty code
function CustomerDetail(User) {
    console.log(`Name: ${User.name} Type: ${User.type} Order: ${User.order}`);
}

// clean code
function CustomerDetail({
    name: CustomerName, // object destructuring with alias
    type: CustomerType,
    order: Order,
}) {
    console.log(`Name: ${CustomerName} Type: ${CustomerType} Order: ${Order}`);
}

CustomerDetail(User);
```

## Nested-ifs

```js
// dirty code
function isUserValid(user) {
    if (user) {
        if (user.age >= 18) {
            if (user.isActive) {
                return true;
            }
        }
    }
    return false;
}

// clean code (guard clauses)
function isUserValid(user) {
    if (!user) {
        return false;
    }
    if (user.age < 18) {
        return false;
    }
    if (!user.isActive) {
        return false;
    }
    return true;
}
```

## Clear Naming

**dirty code**

```js
function hitungLuas(radius) {
    return 3.14 * radius * radius;
}

// clean code
const PI = 3.14; // PI merupakan konstanta
function hitungLuas(radius) {
    return PI * radius * radius;
}
```

## Chaining-then

**Dirty code**

```js
function getUserData(userId) {
    getUser(userId)
        .then((user) => {
            return getUserDetails(user);
        })
        .then((userDetails) => {
            return getUserProfile(userDetails);
        })
        .then((userProfile) => {
            console.log(userProfile);
        })
        .catch((error) => {
            console.error(error);
        });
}
```

**Clean code**

```js
async function getUserData(userId) {
    try {
        const user = await getUser(userId);
        const userDetails = await getUserDetails(user);
        const userProfile = await getUserProfile(userDetails);
        console.log(userProfile);
    } catch (error) {
        console.error(error);
    }
}
```

Jika promise tidak saling bergantung satu sama lain bisa menggunakan Promise.allSettled() agar tidak blocking.

```js
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve, reject) =>
    const DELAY = 100;
    setTimeout(reject, DELAY, "foo")
);
const promises = [promise1, promise2];

Promise.allSettled(promises).then((results) =>
    results.forEach((result) => console.log(result.status))
);

// Expected output:
// "fulfilled"
// "rejected"
```

## Complex Conditional

**Dirty code**

```js
function processOrder(order) {
    if (order.items.length > 0 && order.totalAmount > 0;) {
        sendOrderConfirmation(order);
    }
}
```

**Clean code**

```js
function orderIsComplete(order) {
    return order.items.length > 0 && order.totalAmount > 0;
}

function processOrder(order) {
    if (orderIsComplete(order)) {
        sendOrderConfirmation(order);
    }
}
```

## Hardcoded Value

**Dirty code**

```js
function processStatus(status) {
    if (status === 1) {
        // "1" tidak memiliki arti yang jelas
        // process
    } else {
        // process
    }
}

// hardcoded values pada pagination
const page = 1;
const limit = 10;
const offset = (page - 1) * limit;
```

**Clean code**

```js
const Status = {
    ACTIVE: 1,
    INACTIVE: 0,
};

function processStatus(status) {
    if (status === Status.INACTIVE) {
        return "ERROR";
    }
    return "OK";
}
```

**Clean code pagination**

```js
const DEFAULT_PAGINATION_LIMIT = 10;
const DEFAULT_CURRENT_PAGE = 1;

const page = queryParams.page ?? DEFAULT_CURRENT_PAGE;
const limit = queryParams.limit ?? DEFAULT_PAGINATION_LIMIT;
const PAGE_NUMBER_OFFSET = 1;
const OFFSET = (page - PAGE_NUMBER_OFFSET) * limit;
```
