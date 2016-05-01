#aof-file-parser

A module to read and write .aof files. The aof file format is used by the website [aof.gg](https://aof.gg) ([AnalyzeOrFeed](https://aof.gg)) to store League of Legends replays and some additional metadata.

## Install

```
npm install --save https://github.com/AnalyzeOrFeed/aof-file-parser
```

## Example

```javascript

let aofParser = require('aof-file-parser');

let game = {
    // insert game data here
};

// to write a file
aofParser.save(game, 'replay-euw-123', (result) => {
    if (!result.success) {
        console.log(result.error); // Error, file has not been saved
    } else {
        // File has been saved!
        console.log(result.warnings); // If there were warnings, print them
    }
});

// read a file
aofParser.load('replay-euw-123.aof', (result, replayMetadata, replayData) => {
    if (!result.success) {
        console.log(result.error);
    } else {
        // Replay has been loaded
    }
});

```

## Usage

### .save(game, filename, callback)

#### game: game
Contains all the necessary information about the game.

#### filename: string
The location of the aof file (without .aof ending).

#### callback: (result: {success: boolean, error: string, warnings: Array<string>}) => any
The callback that will be called in case of an error or when the file has been saved successfully.

- If success is false, result.error will contain the error message
- If success is true, result.warnings can contain warnings, but the file will still have been saved

### .load(file, callback)

#### file: string
Location of the .aof file.

#### callback: (result: {success: boolean, error: string}, replayMetadata: replayMetadata, replayData: replayData) => any
The callback that will be called in case of an error or when the file has been parsed successfully.

## Objects

### game

```javascript
var game = {
    regionId: number; // Region id: Can be obtained using our API: https://api.aof.gg/v2/data/static
    gameId: number;
    riotVersion: string; // Version of the current patch. For example: 6.9.1 Can be obtained using our API: https://api.aof.gg/v2/data/static
    key: string; // Encryption key provided by the Riot API
    endStartupChunkId: number;
    startGameChunkId: number;
    players: Array<Players>;
    keyframes: Array<Keyframes>;
    chunks: Array<Chunks>;
}
```

### players

```javascript
var players = {
    id: number; // Summoner ID
    summonerName: string;
    teamNr: number; // 0 for blue or 1 red team
    leagueId: number;
    leagueRank: number;
    championId: number;
    dId: number; // ID of the summoner spell on d
    fId: number; // ID of the summoner spell on f
}
```

### replayMetadata

```javascript
var replayMetadata = {
    version: number; // Version of the .aof file format
    regionId: number;
    gameId: number;
    riotVersion: string;
    key: string;
    endStartupChunkId: number;
    startGameChunkId: number;
    players: Array<Players>;
}
```

### replayData

```javascript
var replayData = {
    keyframes: [{
        id: number; // Keyframe ID
        data: binary; // Keyframe data
    }],
    chunks: [{
        id: number; // Chunk ID
        data: binary; // Chunk data
    }]
}
```
