# games-redis
`gameserverID`, `field` => `value`

We address/index games by the `gameserverid`  assigned to them during creation. 

We can further specify a `field` to retrieve a particular piece of data about a user.

The fields `gameserver-redis` cares about are:

* `id`: randomized string to identify gameservers
* `status`: current status of the container associated with this game. Possible values are:
	* `matchmaking`
	* `initialized`
	* `ready-for-players`
	* `game-started`
	* `game-concluded / fatal error `
* `allocated-players`: the number of players intended to be a part of this game
* `connected-players`: number of players who have connected to the container associated with this game