# players-redis
`token`, `field` => `value`

We address/index players by a `token` assigned to them by the payserver, the first node they interact with.

We can further specify a `field` to retrieve a particular piece of data about a user.

The fields we `players-redis` cares about are:

* `status` the current status of a player at any given point. Possible values are: 
	* pre-pay
	* post-pay
	* pre-game
	* assigned
	* in game
	* win / loss
	* reward-sent (if win)
* `account`: The current [bcoin account](http://bcoin.io/api-docs/index.html?javascript#wallet-accounts) associated with this player
* `gameid`: The current game they’ve been assigned too