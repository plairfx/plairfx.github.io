+++
date = '2025-11-24T13:27:00+01:00'
title = 'Mugura'
draft = false
+++
# Mugura Bridge Backend

## Summary

Mugura is a simple bridge backend that allows you run a [Clef enviroment](https://geth.ethereum.org/docs/tools/clef/introduction) to sign tx's/data safely and set some rules, while the idea was meant to be a simple backend that checks for events emitted by the contracts and fulfills those events.

I had to learn several skills such as how to work with **Docker** and **databases (PostgreSQL & SQL)**, this made it possible to store these events inside a db and store the needed information if an event was not fulfilled instantly.


## Idea

My original idea was to create a backend that checks for events and fulfills the transaction for cross-chain tx, i had come across Clef (signing tool), which made it more secure than normally just storing your private-key locally or on a server.

#### Architecture

For a cross-chain tx (ETH → ARB), there were 4 goroutines running — 2 for each chain:



One was meant to log unfulfilled events and fulfill them in a later loop. When there was not enough balance it would go into the DB, and UnfulfilledEventLogger would stay in the for loop, getting the first value of the DB each time it had new value.

 **Note:** While this caused an issue as other bridge tx's would not be fulfilled if the first bridge tx was not fulfilled, which would cause delays and potential issues.

### Clef 

Now back to the clef signing method, Clef is meant to be a secure enviroment which allows you to sign and set rules.

Both manual signing and signing through rules were possible, meaning:
```javascript
	if (req.transaction.to.toLowerCase() == "YourAddressOrWallet") {
		return "Approve"
	}
```
This will pass or deny depending on the information you pass through the `Rules.js`. Which you have to sha256 and put your masterseed to let it run with your Clef. 

You can read more on this [here](https://geth.ethereum.org/docs/tools/clef/introduction).

Instead of exposing your private-key inside your `.env`, you could use a much safer enviormment and let it run locally on your local machine with or without rules.

---

## What Have I Learned?

Mainly exploring features of golang, like using:
- Goroutines
- Wait groups
- Interacting with a PostgreSQL DB through SQL
- Working with Docker

and also using Clef for the first time!


### Challenges

The whole challenge was using things for the first time (what i wrote above), while some things were a little new like (Using clef).

It was a good learning experience.
