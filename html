// Game Variables
let player;
let otherPlayers;
let sentinel;
let gameState = 'start'
let lightState = 'on'
let lightStateColor = ['green', 'red']
let lightColor = 0
let sentinelIntervalID;

// ==================
// Network Variables
let socket;

// All connected players (used to determine host) 
let connectedPlayers = {};
let playerLastSeen; // = {}; // Track last heartbeat time

// Role management
let isHost = false;

// ==================

// Constants
const gameDuration = 150000;
// ==================

function setup() {
	createCanvas(400, 1080);
	background(200);
	playerLastSeen = {};
	// initalize socket connection
	socket = io.connect($OP.getEchoServerURL(2798752));

	player = new Sprite(200, 940)
	player.color = 'black'
	player.d = 15;
	player.rotationLock = true;
	player.alive = true;

	otherPlayers = new Group();

	sentinel = new Sprite(200, 40)
	sentinel.color = 'green'
	sentinel.d = 50

	socket.on('connect', processConnected);
	socket.on('playerAnnounce', processPlayerAnnounce);
	socket.on('heartbeat', processHeartbeat);

	setInterval(sendHeartbeat, 500);
	setTimeout(determineHost, 1000);
	setInterval(checkForStalePlayers, 1000)

	//Connection Event
	socket.on('connect', function() {
		player.id = socket.id;
		isConnected = true;
		console.log('Connected with ID', player.id);

	});
	// Register a callback function to handle update message
	socket.on('update', updatePlayer)
}

function draw() {
	background(200)
	// Prepare the package
	let package = {
		'x': player.x,
		'y': player.y,
		'id': player.id,
		'name': player.name,
		'time': Date.now
	}

	// Emit update message with package
	if (player.prevY != player.y) {
		console.log("Sending with ID: " + package.id);
		socket.emit('update', package);
	}
	player.prevX = player.x;
	player.prevY = player.y;
	camera.on()
	for (let i = 0; i < 10; i++) {
		fill(i * 20, 200, 200); // blue to gray
		rect(i * 25, i * 100, 400, 40);
	}

	if (kb.pressing('up')) {
		// Player is moving here, so we should check if there's a red light
		if (lightColor === 1) {
			player.delete();
			player.alive = false;
		}
		if (player.alive) {
			player.y--;
		}
	}

	camera.x = player.x;
	camera.y = player.y;
	camera.zoom = 1 - (player.d) / 500
	camera.off()
	sentinel.draw();

	console.log("Player x: " + player.x);
}

function processConnected(sock) {
	myId = socket.id;
	connectedPlayers[myId] = Date.now();
	console.log('=== CONNECT ===');
	console.log('Connected with ID:', myId);
	console.log('My timestamp:', connectedPlayers[myId]);

	// Announce our presence
	socket.emit('playerAnnounce', {
		id: myId,
		timestamp: connectedPlayers[myId]
	});
	console.log('Sent initial playerAnnounce with timestamp:', connectedPlayers[myId]);
}

function processPlayerAnnounce(data) {
	console.log('=== RECEIVED playerAnnounce ===');
	console.log('From ID:', data.id);
	console.log('With timestamp:', data.timestamp);
	console.log('Is me?', data.id === myId);

	// Track all players
	if (data.id !== myId) {
		// This is a new player we haven't seen before
		let isNewPlayer = !(data.id in connectedPlayers);

		connectedPlayers[data.id] = data.timestamp;
		playerLastSeen[data.id] = Date.now(); // Initialize last seen time
		console.log('Stored player', data.id, 'with timestamp:', data.timestamp);

		// When we hear a new player, announce ourselves so they know about us
		if (isNewPlayer) {
			console.log('NEW PLAYER DETECTED! Re-announcing myself...');
			socket.emit('playerAnnounce', {
				id: myId,
				timestamp: connectedPlayers[myId]
			});
		}
	}

	console.log('Current connectedPlayers:', JSON.stringify(connectedPlayers, null, 2));
}

function sendHeartbeat() {
	if (socket.connected) {
		let package = {
			id: myId,
			timestamp: Date.now()
		};

		// If I'm the host, add the lightColor (i.e. sentinel status) to the heartbeat package
		if (isHost == true) {
			package.lightColor = lightColor;
		}
		socket.emit('heartbeat', package);
		console.log('Sending light color: ' + lightColor)
	}
}

function processHeartbeat(data) {
	if (data.id !== myId) { // If it's someone other than me, update their "playerLastSeen" timestamp
		playerLastSeen[data.id] = data.timestamp;
		console.log('Heartbeat from', data.id);

		// If there is a lightColor (which means we are not the host), update our copy with this status
		if (data.lightColor != null) {
			lightColor = data.lightColor;

		}
	}
}

function checkIfHost(playerId) {
	let hostId = null;
	let earliestTime = Infinity;

	for (let id in connectedPlayers) { // Go through the list of all connected players
		if (connectedPlayers[id] < earliestTime) { // Does this one have an earlier time than the current earliest time?
			earliestTime = connectedPlayers[id]; //If yes, update the earliest time and set that player as the host
			hostId = id;
		}
	}

	return hostId === playerId; // true if the passed "playerId" is the host, false otherwise
}

function determineHost() {
	console.log('=== DETERMINE HOST ===');
	console.log('All players:', JSON.stringify(connectedPlayers, null, 2));

	let hostId = null;
	let earliestTime = Infinity;

	// Find the player with earliest timestamp
	for (let id in connectedPlayers) {
		console.log('Checking player', id, 'with timestamp', connectedPlayers[id]);
		if (connectedPlayers[id] < earliestTime) {
			earliestTime = connectedPlayers[id];
			hostId = id;
			console.log('  -> New earliest! Host is now', id);
		}
	}

	let wasHost = isHost;
	isHost = (hostId === myId); // Store whether I am the current host in "isHost"

	console.log('RESULT: Host ID is', hostId, '| I am', myId, '| isHost =', isHost);
	if (isHost == true) {
		sentinelIntervalID = setInterval(switchLight, 5500);
		console.log("sentinelIntervalID is " + sentinelIntervalID);
	}

}

function switchLight() {
	lightColor++;

	if (lightColor >= lightStateColor.length) {
		lightColor = 0;
	}
	console.log("Light color is " + lightColor);
	sentinel.color = lightStateColor[lightColor];

	// Reset the interval semirandomly
	// First we "clear" the existing interval, which stops the timer
	clearInterval(sentinelIntervalID);
	// Then, we set a new interval and store it in the same variable, which allows us
	// to continue clearing and resetting it
	sentinelIntervalID = setInterval(switchLight, random(3000, 6000));
}

function updatePlayer(b) {
	if (b.id === player.id) {
		return;
	}

	let bs = otherPlayers.filter(oP => oP.id === b.id);
	let matchedSprite = bs[0];

	if (bs.length == 0) { // If a new player, create sprites and a joint
		console.log("Creating new player with id: " + b.id)

		// Create the player's sprite
		let tempPlayer = new otherPlayers.Sprite(random(10, 200), 940);
		tempPlayer.id = b.id;
		tempPlayer.color = color(0, 120);
		tempPlayer.d = 15;
		tempPlayer.alive = true;
		tempPlayer.rotationLock = true;
		tempPlayer.timeLastUpdate = Date.now()
	} else {
		matchedSprite.y = b.y;
		matchedSprite.timeLastUpdate = b.time;
	}

}

function checkForStalePlayers() {
	// Check for stale players every second
	let now = Date.now();
	let removedAny = false;

	for (let id in playerLastSeen) {
		if (now - playerLastSeen[id] > 3000) { // 3 second timeout
			console.log('Player', id, 'timed out - removing');
			delete connectedPlayers[id];
			delete playerLastSeen[id];
			removedAny = true;
		}
	}

	if (removedAny) {
		determineHost(); // Re-determine host if anyone was removed
	}
}