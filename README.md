# Sequencer Database Entries (SDBE)

A utility module for Foundry VTT that automatically scans a designated assets folder and registers the contents into the Sequencer database. No more manual JSON indexing—just point to your folder and sync.

## Features

* Automatic Indexing: Recursively scans your Foundry data folders for supported media files.
* Sequencer Integration: Automatically formats and registers discovered assets using Sequencer.Database.registerEntries.
* One-Click Refresh: Includes a dedicated "Build Database" button in the module settings to sync new files without restarting the server.
* Persistent Storage: Stores the indexed database in world settings so it is available at world load for all users.
* Import/Export: Easily backup and restore your database, or share it between worlds.

## Configuration

1. Open Configure Settings in the Game Settings tab.
1. Navigate to the Sequencer Database Entries section.
1. Click 'Build Database'.
1. In the prompt, set an asset filepath.
1. Click 'Build Database' to build the database.
1. Reload when prompted.
1. Enjoy!

![Setup Tutorial](./assets/demo.gif)

## Developer Features

### Import / Export Database for Module Development

A powerful feature of SDBE is the ability to export the generated database to a JSON file. This is hopefully for developers of asset modules.

You can pre-build a database of your module's assets and utilize the resulting JSON file with your module providing quick Sequencer support.

1. Build and export your SDBE database
1. Modify this by hand however you'd like
1. Import to verify the new functionality
1. Add the following code to your module with the JSON you're happy with

```js
    const SEQUENCER_DATABASE_ID = 'my_module_sdbe_id';
    const SEQUENCER_DATABASE = { /* Exported JSON information */ };
    if (SEQUENCER_DATABASE && Object.keys(SEQUENCER_DATABASE).length > 0)
        Sequencer.Database.registerEntries(SEQUENCER_DATABASE_ID, SEQUENCER_DATABASE);
```

**Workflow:**

1.  Follow the steps in the **Configuration** section to build a database from your module's asset folder.
2.  In the module settings, click **Export Database**. This will save a JSON file (e.g., `sequencer-database-backup.json`).
3.  Rename this file to something descriptive (e.g., `my-asset-pack.json`) and include it in your module.
4.  Instruct your users to use the **Import Database** button to import your JSON file.

This provides a seamless experience for your users and ensures that your assets are correctly indexed in Sequencer.

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
   - Replace all trailing digits with a three digit subindex: `my.fireball11.webm` will be indexed to `sdbe.my.fireball.011`


 * Indexing rules apply to both folder name and path names
   - `my cool/fire ball.webm` will be indexed to `sdbe.my.cool.fire.ball`

### Conflicts
 * Conflicts (Try to avoid these... - relatively untested and 100% janky):
   - `my/fireball.webm` and `my.fireball.webm` and `my fireball.webm` will all be indexed to `sdbe.my.fireball`.
   - `my/fire/ball/blast.webm` and `my.fireball.webm` will have a conflict at the path `my.fireball` since the former expects this to expand out, the latter expexts this to be an index to a string path.
 In both cases the module will try to "fix" this by indexing them further with `000`. This is a janky solution to a problem of misuse... 
 
 **Moral of the story:** Just name your files something different so they don't conflict.
   

## Requirements

* Foundry VTT v13+
* Sequencer: Must be installed and active.

## 📄 License

This module is released under the MIT License.

