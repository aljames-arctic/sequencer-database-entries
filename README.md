# Sequencer Database Entries (SDBE)

A utility module for Foundry VTT that automatically scans a designated assets folder and registers the contents into the Sequencer database. No more manual JSON indexing—just point to your folder and sync.

## Features

* Automatic Indexing: Recursively scans your Foundry data folders for supported media files.
* Sequencer Integration: Automatically formats and registers discovered assets using Sequencer.Database.registerEntries.
* One-Click Refresh: Includes a dedicated "Build Database" button in the module settings to sync new files without restarting the server.
* Persistent Storage: Stores the indexed database in world settings so it is available at world load for all users.

## Configuration

1. Open Configure Settings in the Game Settings tab.
1. Navigate to the Sequencer Database Entries section.
1. Set and save an asset filepath.
1. Reopen the Sequencer Database Entries Configuration Settings
1. Click 'Build Database' to build a database.
1. Reload when prompted.
1. Enjoy!

![Setup Tutorial](./assets/Setup%20Tutorial.gif)

## Usage

Once the world is refreshed with your build Sequencer Database, your files will be available via the Sequencer database.

```js
new Sequence()
    .effect()
        .file("sdbe.path.to.your.file")
        .atToken(token)
    .play()
```

## Helpful Tips

### Indexing
 * In general, the database structure mimics your folder structure. 
 
 For example, a file at `assets/spells/my_fireball.webm` will be indexed to `sdbe.spells.my_fireball`.

 * Indexing Rules
   - Replace all spaces with a subindex: `my.fireball.webm` will be indexed to `sdbe.my.fireball`
   - Replace all / with a subindex: `my/fireball.webm` will be indexed to `sdbe.my.fireball`
   - Replace all \ with a subindex: `my\\fireball.webm` will be indexed to `sdbe.my.fireball`

 * Indexing rules apply to both folder name and path names
   - `my cool/fire ball.webm` will be indexed to `sdbe.my.cool.fire.ball`

### Conflicts
 * Conflicts (Try to avoid these... - relatively untested and 100% janky):
   - `my/fireball.webm` and `my.fireball.webm` and `my fireball.webm` will all be indexed to `sdbe.my.fireball`.
   - `my/fire/ball/blast.webm` and `my.fireball.webm` will have a conflict at the path `my.fireball` since the former expects this to expand out, the latter expexts this to be an index to a string path.
 In both cases the module will try to "fix" this by indexing them further with `01`. This is a janky solution to a problem of misuse... 
 
 **Moral of the story:** Just name your files something different so they don't conflict.
   

## Requirements

* Foundry VTT v13+
* Sequencer: Must be installed and active.

## 📄 License

This module is released under the MIT License.

