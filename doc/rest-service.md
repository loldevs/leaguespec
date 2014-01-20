# REST Service for LoL spectators
This is an unofficial, uncomplete and (pretty sure) wrong documentation of the RESTful service which powers the League of Legends spectator mode.

This documentation is desgined to be community driven and should be extended by everyone. If you find things missing, add them please!

## How it works

Riot's spectator mode works by requesting replay data via HTTP form a service.
The data is split in chunks which usually contain about 30 seconds of gameplay. Additionally there are key frames which seem to contain
more information then a single chunk. They seem to be used to support
skipping back in time or for spectators that join the game later.
This means that the key frames should contain every information about
the state of a game at a given point in time. On the other hand, chunks may only contain changes to this state.

This leads to the following process for replaying a series of chunks:

1. Read key frame thats prior to the targeted point in time
2. Read chunk following this key frame
3. apply the changes described in the chunk
4. read next chunk
5. repeat 3 + 4 until end of game or skip

The same procedure is known to work in many video and audio formats. It provides a fast and bandwidth saving way of deliviering all information fast while it also gives a good way to jump quickly to any point in time without the need to replay every chunk from the start to that moment.

## Cryptography

Chunks and Keyframes are encrypted and compressed. The algorithm used for cryptography seems to be Blowfish in ECB mode. Compression is done using zip BEFORE the encryption. So to read a chunks data one needs to first decrypt then uncompress the data.

## API Docs

### URL Template

Most of the REST URLs look like this:
```
http://<REST host:port>/observer-mode/rest/consumer/<method>/<platformId>/<gameID>/<parameter>/token'
```
**Parameters**

* **host:port**: there are currently 8 REST services running for the different regions. A list of these servers exists at the end of this document.
* **method**: Name of the method you want to call. See "*Methods*".
* **platformId**/**gameID**: Every game is uniquely identified by the short name of its region and a numeric game ID.
* **parameter**: The methods first and single parameter (aside of gameid/platformId). If a method doesn't require an additional parameter this value is '1'

### Methods

#### featured() > JSON

URL **.../featured**

Lists the 10 featured games for the regions supported by this server.

### version() > text/plain

URL: **.../version**

Contains the current version for this Region.

#### getGameMetaData(platformId, gameId) > JSON
URL: **.../consumer/getGameMetaData/&lt;platformId&gt;/&lt;gameID&gt;/1/token**

Returns information about the given game. This contains the games type and map, summoners involved, champions picked & banned, start time of the game and the encryption key required to read the replay data.

#### getLastChunkInfo(platformId, gameId) > JSON
URL: **.../consumer/getLastChunkInfo/&lt;platformId&gt;/&lt;gameID&gt;/1/token**

Return some information about the last avaiable chunk:

* **chunkId**: ID of the last available chunk,
* **availableSince**: Number of ms this chunk is available (?),
* **nextAvailableChunk**: Number of ms until the next chunk is available (?),
* **keyFrameId**: key frame that belongs to this chunk,
* **nextChunkId**: chunk that directly follows this key frame,
* **endStartupChunkId**: chunk that determinates the end of pick&ban phase,
* **startGameChunkId**: fist chunk of the actual game,
* **endGameChunkId**: last chunk of the actual game (or 0 if game still running),
* **duration**: Number of ms this chunks information represents

#### endOfGameStats(platformId, gameId) > base64 encoded AMF Data
URL: **.../consumer/getLastChunkInfo/&lt;platformId&gt;/&lt;gameID&gt;/null** (!)

Contains data used for the statistics screen after a game.

Example Content: http://pastebin.com/KB4TUPhs (thanks to [Divi](https://github.com/Divi))

#### getGameDataChunk(platformId, gameId, chunkId) > ? Encrypted, Compressed
URL: **.../consumer/getGameDataChunk/&lt;platformId&gt;/&lt;gameID&gt;/&lt;chunkId&gt;/token**

Retrieves a chunk of data for the given game.

#### getKeyFrame(platformId, gameId, keyFrameId) > ? Encrypted, Compressed
URL: **.../consumer/getKeyFrame/&lt;platformId&gt;/&lt;gameID&gt;/&lt;keyFrameId&gt;/token**

Retrieves a key frame for the given game.

## Plattforms
* NA
* EUW1
* EUN1

## Server Names & Ports
* **NA**:  spectator.na.lol.riotgames.com:8088
* **EUW**:  spectator.eu.lol.riotgames.com:8088
* **EUNE**:  spectator.eu.lol.riotgames.com:8088
* **BR**:  spectator.br.lol.riotgames.com:80
* **LAN**:  spectator.br.lol.riotgames.com:80
* **RUS**:  spectator.tr.lol.riotgames.com:80
* **TUR**:  spectator.tr.lol.riotgames.com:80
* **PBE**:  spectator.pbe1.lol.riotgames.com:8088
* **SK**:  QFKR1PROXY.kassad.in:8088
* **TW**:  QFTW1PROXY.kassad.in:8088
* **SEA**:  qfsea1proxy.kassad.in:8088

## Sources

* https://github.com/robertabcd/lol-ob/issues/2#issuecomment-25749662
* https://gist.github.com/lukegb/d2997a5fc7970ce6e1e1
* https://github.com/robertabcd/lol-ob/wiki/ROFL-Container-Notes
* https://github.com/robertabcd/lol-ob/blob/master/rofl.rb
