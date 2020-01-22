---
title: "But Await, There's More!"
date: 2020-01-28T23:30:00.000Z
description: >-
  Common patterns with Promises and async/await in JavaScript
---

Asynchronous programming is hard. This post aims to clear up a few common patterns you might see when writing asynchronous code. It's not a comprehensive primer of Promises or async/await, but instead provides some practical examples that cover a few common use cases for asynchronous code.

## Background

For these examples, let's assume a few common pieces of code:

```
/**
 * Asynchronous function to load a single account by ID. Returns a Promise that
 * resolves to an Account object
 */
const loadAccount = async (accountId: number): Promise<Account> => {
  console.log(`Loading account ${accountId}`);
  const account = await makeNetworkRequest...
  console.log(`Loaded account ${accountId}`);
  return account;
}

const loadTransactions = async (accountId: number): Promise<Transaction[]> => {
  console.log(`Loading transactions for account ${accountId}`);
  const transactions = await makeNetworkRequest...
  console.log(`Loaded transactions for account ${accountId}`);
  return transactions;
}
```

These functions are both `async`, meaning they both return `Promise` objects that will either reject with an error or resolve with the data we requested.

## Concurrent Requests

```
const accountId = 1;

const [account, transactions] = await Promise.all([
  loadAccount(accountId),
  loadTransactions(accountId)
]);

console.log('Loaded both accounts and transactions');
```

Output:

```
# Both requests start at the same time
Loading account 1
Loading transactions for account 1

# These next two lines may be in either order, depending on which network request finishes first
Loaded account 1
Loaded transactions for account 1

# Always logged after both requests finish
Loaded both accounts and transactions
```

### What just happened?

We started both of these functions at the same time (run them in parallel), but waited until they both finished before continuing past the Promise.all.

### When would I use this?

If you have two or more network requests that are independent, but you need to make sure they all finish before you continue on, this pattern is well-suited.

## Sequential Requests

```
const accountId = 1;
const account = await loadAccount(accountId);
const transactions = await loadTransactions(accountId);
console.log('Loaded both accounts and transactions');
```

Output:

```
# Load account
Loading account 1
Loaded account 1

# Then load transactions
Loading transactions for account 1
Loaded transactions for account 1

# Always logged after both requests finish
Loaded both accounts and transactions
```

### What just happened?

By listing the two function calls on two lines and putting `await` before both of them, we're ensuring that they run sequentially (one after the other). We then continue on after both have finished

### When would I use this?

If you have two or more network requests that are dependent, and you need to make sure they all finish before you continue, this pattern will work.
For example, if loadTransactions used a field from the account object in its request, we would need to make sure loadAccount finished before calling loadTransactions

## A little bit of both

```
const accountIds = [1, 2, 3];
const accountsAndTransactions = await Promise.all(
  accountIds.map(async accountId => {
    const account = await loadAccount(accountId);
    const transactions = await loadTransactions(accountId);

    console.log(`Finished account ${accountId}`);
    return {
      account,
      transactions
    };
  });
);
log.info('Loaded all accounts and transactions');
```

### Output

```
Loading account 1
Loading account 2
Loading account 3

# Log messages may be interleaved because we're executing each iteration of the map function concurrently.
Loaded account 1
# "Loading transactions for account 1" will always be after "Loaded account 1" because of the `await` keywords inside the map function
Loading transactions for account 1

Loaded account 2
Loading transactions for account 2

Loaded transactions for account 1
Finished account 1

Loaded transactions for account 2
Finished account 2

Loaded account 3
Loading transactions for account 3
Loaded transactions for account 3
Finished account 3

Loaded all accounts and transactions
```

### What just happened?

We iterated over all of the account IDs with the **map** function. For each account ID, we're executing an async function, and we're wrapping the whole thing in a Promise.all.
The result of this is that we start executing the map function for each account ID concurrently.

Inside the body of the map, we're executing **loadAccount** and **loadTransactions** sequentially for a single account ID. So each execution of the map function will not finish until we've loaded both the account and the transactions for that specific account ID.

The end result is that we're starting the load for each accountId concurrently, making sure that it runs its internal requests sequentially, and then continuing on at the top level when we have accounts and transactions for every accountID.

### When would I use this?

If we have an array, and we want to execute the same code for each item, we can do so with **map**. If each item in the array can be loaded independently, we can use **Promise.all** to run them concurrently.

Inside the body of the map function, we may want to execute things sequentially for that particular item in the array. This is similar to the sequential case, but for the case where we have several account IDs and want to load them all.
