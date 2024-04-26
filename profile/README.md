# NixuCraft
Welcome to NixuCraft

This repo contains a bunch of plugins/libraries/tools to have a get a basic Minecraft network running

### For now things on here are nowhere complete
The biggest focus of this is to be portable and easy to setup. All you have to do is edit a few json/yml configs and provide server templates, and you have something that works.

Also note that I made this organization mostly to not clog up my personal account with Minecraft things related to this.

## Bit more complete setup guide:
#### ServerManager 
- Download the serverMgr
- Fill in the config files under config/games/ for all your games (follow the examples that are there by default. For now pushing my configs directly, will change once this thing is stable enough)
- Fill in the lobby configs under config/lobby/ (same as game configs)
- Provide server templates in cache/servers/, maps in /cache/maps/<game>/ and plugins in /cache/plugins/
- Provide lobby instances in instance_lobbies/ (NOTE: UNLIKE TEMPLATES WHICH ARE COPIED TO A NEW DIRECTORY EVERY TIME A NEW ONE GETS CREATED, THE LOBBY INSTANCES ARE USED AS IS, THAT MEANS THEY CAN CHANGE THE WORLD AND EVERYTHING)

#### LobbyPlugin
Most things here should be self explanatory. Here are some important notes still:
- All games provided in the rooms.yml SHOULD ALSO BE IN THE SERVERMANAGER, otherwise you'll get an error.
- lobbyGames can either have instances (jump, pvparea, eg things that have specific areas bound to them) or no instance (snake, plays in a gui), just see the default config & you'll figure it out
- You technically don't need anything in gui.yml, altho it's obviously recommended for players to have a better experience.

#### Velocity
Simply building & loading the VelocityCore on velocity should be enough. Note that the ServerManager NEEDS to be running when running Velocity.

#### Game servers
If you managed to get both the servermanager & lobbyplugin up and running congrats, you're most likely 99% done. 
All the game servers do is handle themselves, if you configured the templates right it should just work as-is.

## Making your own game plugins.
- Include (implements) the VelocityHandler project in your Gradle build & make a new instance of it inside of onEnable (eg: `new VelocityHandler(this, "gameserver")`)
- You should be able to send plugin messages (see the "messages" folder in the VelocityHandler project), some of which can have callbacks (See messages/RequestNewServerMessage)
- Register the game in the ServerManager (& in the LobbyPlugin's rooms.yml and optionally gui.yml) as said above


## Obvious limiations
- Everything is a bit "janky", in a way that makes it easy to config but also easy to throw errors (fortunately mostly errors that get caught at startup, but not all)
- Limited to 1 proxy (won't change anytime soon)
- Limited to 1 machine (will change soon, at least by allowing to start servers through ssh remotes. Still underoptimal kinda, but easy and if the servermanager itself is stable should be error proof)
- No 1.6 :(
- Dependencies are kinda messy, ideally the core plugin (when it's done) should include the basic always required libraries (eg the VelocityHandler) while the game plugins should include other libs (and have required libraries as compileOnly)
- Velocity itself is handling the server requests/switches. That's not really a problem, except for the fact Velocity can't handle lobby swaps, eg if you restart the ServerManager it'll keep the old servers (including the old lobbies), meaning you'll also have to restart Velocity
- On top of the previous point, there's no way to add games on the fly (=without killing every running server)

## Missing stuff
- Stats
- Core plugin (/msg, /party, /spec, moderator commands...)
