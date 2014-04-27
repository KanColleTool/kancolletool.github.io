---
layout: post
title:  "The Spring patch for Developers"
date:   2014-04-27 15:42:00
categories: kancolletool releases
---

The latest KanColle patch merges a bunch of endpoints together to speed up the game, and makes an earnest effort to make tool development harder. Of course, this isn't gonna stop me, nor anyone else, so here's what's changed.

### No more `/api_get_master/`

Yeah, that's right, no more `api_get_master`. Instead, all authoritative data is in the new `/api_start2` endpoint.

`/api_get_master/ship` is now `/api_start2` -> `api_data.api_mst_ship`, `/api_get_master/slotitem` is `/api_start2` -> `api_data.api_mst_slotitem`, etc.

### Random ship data filenames

Ship graphics used to be named simply `<Ship ID>.swf`; ever since the spring patch, they have a random sequence of letters for filename instead. These filenames can be pulled from the `/api_start2` endpoint, under `api_data.api_mst_shipgraph`.

### Player data merged into `/api_port/port`

This is perhaps the most interesting change: a bunch of player data is now under one endpoint, but that endpoint also requires a signature in the `api_port` parameter. Here's the code for how to generate that signature from the player's `member_id` (you can grab this from `/api_get_member/basic`), in JavaScript, because the game does it in ActionScript which is almost the same thing:

```javascript
// u = The current user's ID
function _(u) {
	var loc = [1171, 1841, 2517, 3101, 4819, 5233, 6311, 7977, 8103, 9377, 1000]
	var ret = 
		(loc[10] + u % loc[10]).toString() +
		((9999999999 - Math.floor(new Date().getTime() / loc[10]) - u) * loc[u % 10]).toString() +
		cN() + cN() + cN() + cN();
	return ret;
}

function cN() {
	return Math.floor(Math.random() * 10).toString();
}
```

As you can see, this isn't one number, but instead three parts of fixed lengths.  
The server *will* verify that this signature is correct, and if you get it wrong, you get Error 100 back.
