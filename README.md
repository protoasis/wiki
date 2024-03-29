# ProtOasis
ProtOasis is a 3D first-person sandbox role-play game with decent graphics, smooth rolling ground and buildings.  Server owners & modders would be able to create new assets (such as streets, roads, vehicles, etc)

There is a 3-tier layer to the server, the first being a "lobby" area, a world that looks much like a neighborhood, or a city, with players able to claim property on the roads (or in the middle of nowhere)  There will be warps (near instant), fast travel (10-100x the speed of walking), mounts (2x speed of walking) and of course walking to get around the city.

When a player enters a property, they are sent to that player's instance.  Inside the player's instance, is a small world all of their own.  The players will have security mechanisms to prevent unwanted guests. Staff, however, will be able to override these restrictions for security and rules enforcement purposes.

Inside these instances can be dungeons - smaller instances that will allow players to have private areas, fighting arenas, roleplay rooms, what ever they want.  These dungeons (for lack of better word) will have their own security, allowing the player to open their instance to the public, but prevent access to their dungeons.

The size of the player's instance and the number of dungeons they may create will be limited, but expandable by some means (donations, likely, or possibly something related to visitor counts)  The entrances to these dungeons may look like doors, holes, caves, or even moving objects such as balls floating in the air. 

The main world will also have public areas where players can go to host RPs, fight mobs, PVP, etc.  There would be something like schools, libraries, parks, forests, sewars, etc. These would be party-instanced, and so multiple parties could be in the same building without interfering with each other. There would be an invite system where players can invite their friends to their party, regardless of where the party is - but the player invited would need to make their way to the party.

## Technical Aspects
ProtOasis will consist of at least these software packages: client-side, server-side, and utility.
*(Entries marked with a number are primary projects, marked with ? are questionable)*
### Client-side
 * **ProtOasis Signal (1)** - non-graphical client for mobile and desktop
   * Query (ProtOasis Signal: Red)
   * (1) text & query (ProtOasis Signal: Yellow)
   * (?) voice, text & query (ProtOasis Signal: Green)
 * **ProtOasis Rails (2)** - graphical (3D) client
 * **ProtOasis Platform (?)** - 2D overhead graphical client
 * **ProtOasis Conductor (3)** - Staff accessibility module for Rails & Platform (permission based usage)
### Server-side
 * **ProtOasis Depot (1)** - Signal Yellow/Green compatible development/dummy server
 * **ProtOasis Station (2)** - main game server built upon Depot
 * **ProtOasis Junction (4)** - proxy server for connecting clients to servers
### Utility / Administrative Tools
 * **ProtOasis Terminal (1)** - console, local and remote. 
 * **ProtOasis Engineer (3)** - Asset editor and unlocked creative-mode client (permission based usage)
 * **ProtOasis Expo (5)** - an asset and resource market place and licensing control authority

## Network Topography
### Initial connections
Clients (Signal, Platform and Rails) will connect to one gateway service (Junction, Depot or Station) When using Junction, clients will be forwarded to a default Depot or Station for the community, as assigned in the Junction config.
Junction will allow Depot and Station server connections.

### Connection transfers
Clients will connect to Junction and be forwarded to the community's default Depot or Station.  If the community has multiple Depots or Stations connected to their Junction, then Junction will transfer the client connection between these servers as needed.

A server-server branching hierarchy will allow communities to run multiple servers, directly connected to their primary Station. The primary Station will act as a connection hub, allowing those players to switch servers without Junction changing their connection.  Server-Server connections can be made between Junctions, Stations and Depots, and a mix.  Specifically, for client connections:
Junction-Junction (eventually ending in a Depot or Station)
Junction-Depot (dead-ended at Depot)
Junction-Station (can then forward to other Stations)
Station-Station

Depots may connect to other Depots and Stations, and vice-versa, for sharing of text and voice chat only.

The Station-Station branching topography will allow for spinning up new server instances off the main instance, to allow for the seamless expansion of the main server - Station-Station connections should be hearty for remote connections, as player connections will not be transferred away from the main Station, but will go through it.

## In-game Topography
The client will connect to the default/main Station, and be presented with a visualized world.  This world will be in-essence a giant, glorified hub server.  Within this world will be a landscape of varying themes. (An example here: such as an old-west town, with a train station (the main spawn area), main street, saloon, hotels, general store, etc. Away from the town would be ranches and farms.  Another example would be a futuristic city theme, where the players would spawn in/at a mag-lev metro station, surrounded by high-rise buildings.)  In this world, players would be able to claim (either for free or for purchase) a plot of land, floor in a building, etc.  It would be up to the server operators to define claimable zones in a 3D space (2D clients would use Z shifting to determine level) 

Upon claiming/entering one of these areas, the player is transferred to an instance which is spawned by the Station, or transferred to another Station hosting that instance.  This is done with zone portals, which are only active once the zone has been claimed. Zone portals may be re-shaped and re-sized by the player, for example, to force the use of the front door as the zone portal.  Zones can be decorated in one of two ways: The operator pre-defines the aesthetics, such as with the city buildings; or the zone can be represented by a miniature snapshot of the player's build within their zone.  (Clarification: If main world zone decoration is set to player-supplied, what ever the player builds within their zone instance will be displayed for their zone in the main world, effectively a thumbnail of their build)

Likewise, there will be several different regions and types of zones:
Regions:
  Instanced - part of the same Station server
  Local - another Station as part of the community (behind the same Junction)
  Remote - another Station hosted behind another Junction (meaning, the player's connection leaves the initial Station, and is relayed to the appropriate Junction for forwarding to the correct Station server)
Types:
  Player - These are zones where the player has control over the zone, their own space on the game
  Public - These are zones where the players may go, and have varying levels of interaction
  Admin - Zones for which only administrative staff may join. Useful for "Admin Office" or creating new Public zones.
  Community - Though no different than Public in use, these zones are previous Player Zones which have been opened up to the public - certain attributes are retained from Player, and creator credits are supplied. 

Sub-types:
  World - the main world, These are public zones, but with the intention of hosting zone portals to useful areas.
  Dungeon - mini zones, has no zone portals (with exception of an exit), and may be accessed through Player, Public, Community or Admin zones.  These will not be accessible directly from World zones. Dungeons are apart of the zone hosting the zone portal, and cannot be separated.  Think of dungeons as compartments within zones. 

## User Accounts
Players will be required to register an account.  This account will utilize UUIDs, and allow for name changes. There are limited restrictions on names, allowing for explicit and duplicate names.  It is up to the server operators to implement name blacklisting, or alt-name forcing.  Names should only use natural language characters and non-interfering ascii symbols (-, _, ^, <, >, [, ]), i.e. no emoji or emoticons. Names using non-latin characters are allowed but must begin with one of the 64 characters of ASCII/Latin list. UTF-8 support will be implemented for chat, however. 
Names will contain 3-16 characters.  Alt-names (as defined by server operators) may use 1-32 characters, using any character in the URF-8 table.  Alt-names & nicknames will be handled by Junction, or Station. Alt-names are defined by the server operator whilst nicknames may be enabled and defined by the player. 

Name, Alt-name or nickname: the player will always be identified and handled by their UUID. 

## Server Security Functions

### Zone Trust
Players will be able to privatize their zones, allowing in only those players/groups they choose, or allowing everyone or no-one. This is the Zone Trust system.  This will handle all Zones as well as Dungeons.  Zones default behavior will depend on the type of zone, with World, Public and Community zones allowing everyone to enter and use Zone Portals.  Zone Trust can then be set to allow certain players or groups to have more or less privileges.  The Admin zones will not allow players without the Admin group to enter.  Player zones will give full zone control to the zone owner, who can then trust how ever they would like. 

This is a simple system, allowing players to easily configure their zones, and will have at minimum these options:

**For player/group settings:**
 * Entry
 * Interact
 * Move (move items around in the zone)
 * Build
 * Destroy
 * Portal (to use Dungeon portals)
 * Loot (to pick up dropped items) 

**For zone settings:**
 * Chat
 * Voice
 * Fly
 * PvP
 * PvE
 * Gravity (0-10, with 0 being weightless, and 10 being walking only, no jumping, default of 5)
 * Gamemode (Creative or not, which states if the zone has access to the creative inventory or not. Creative zones have special asset tracking)

Players will be able to invite, mute, kick and ban other players in regards to their zone. 

### Server Groups
There will be a set of groups which are super to all other groups.  Each person will fall into one of these groups:
 * Operator - will have access to the console, remote or local, and has full control over every aspect of Junction or Station
 * Admin - Will have access to Admin zones, promoting persons up to Staff, and can bypass Zone Trust restrictions
 * Staff - Will have access to control players for rules violations, but will abide by Zone Trust restrictions
 * Player - normal set of permissions, can be loosely defined by server operator
 * Banned - any UUID in this group will be prohibited from joining the Junction or Server

### Permission System
Modules and Plugins may be permissionable, and as such, a separate Permission Management System will be needed to allow for fine-grain control over the permissions.  These permissions will be applied to the Server Groups, but will not override, nor provide any of the functionality of Server Groups.  Meaning, the PMS cannot give Admin level permissions. 

## Social Groups
Think of this as a Party or Factions system. The group leader will be able to define various levels within the group. This allows for players to form groups, and for which groups may be given permissions in Zone Trust.  Players may form 1 group, however may be a member of multiple Social Groups. Group allegiences may be formally formed, with the primary group assigning their allies to a specific level within their own group. (CoolKids may ally with SlickKids, and then assign all of SlickKids to CoolKids' level 2, which will give SlickKids the same Zone Trust levels as any member in CoolKids' level 2.  Member of SlickKids who hold a higher level in CoolKids will retain that level)

## Technical Aspects
Primary programming languages:
C, C++ & C# (C# should be limited to ancillary code) 
There may be secondary languages used, for purpose specific aspects which either need the functionality or dev time these languages provide. 

### Scripting languages
These are intended for server administrator and player use to set up scripts to use pre-existing functionality, NOT to add new functions, features, etc. (This will be abused, I am sure, never-the-less, it is stated here)

**Server-side:**
 * MethodScript
 * Lua
 * Ruby
 * Python
 * JavaScript

**Client-side:**
 * JavaScript
 * Lua

### Modules/Plugins languages: 
These will add new functions, features, protocols, etc to the server and client. 

Server: C/Cpp/C# with the ability to call/use scripts and APIs written in other languages.

Client: Let's start with C/Cpp/C#, and JS for client modules/plugins. We can add more as needed later.

A module is a base game changing component that is loaded at start.  A plugin, however, should be able to be hot-loaded, and hot-unloaded from the server and client.  Scripts are real-time and can be enabled/disabled as needed by the server admins and players (client-side)

### Data Storage
The primary data storage will be SQL, using MariaDB and Microsoft SQL Server. 
Configuration files should use conf (standard key-pair) and toml for human-readable, low-complexity files;  JSON and yaml can be used for complex config files - yaml being more human readable, json being for things humans should never have to mess with.

The world will be stored completely in SQL.  Links to zone entries can be used to move zones around. However, it may be required to copy the zone and world data from one SQL server to another.  There will be a coordinates system.  Because of the potential of eventually connecting Stations together in a way which will form, more or less a solar system of Stations, coordinates will may include a 4th dimensional (w) coordinate. `w*x*y*z`  - AND Z WILL BE ALTITUDE! 
There will be a cardinal direction system as well, and North will be +y, and West will be -x. The coordinate system will be based on World, Longitude, Latitude, and Altitude.  This should allow worlds of any size.  The world will be stored as a flat world, but the point where -x and +x ends, will form a seamless world where when displayed and interacted. This can be done with dynamically loading the world segments, as needed to the player's client.  As this is always done, there will be no seams between the left and right most edges of the flat map.  The same can be done on the x and y axis. We can then use GIS server storage schematics for the worlds. 

### Networking
ProtOasis uses several transport layer protocols.
For client-server meta info, UDP (implementing UT3 query, or similar)
For client-server and server-server error messaging, ICMP
For client-server data transmission, TCP  (later, I would like to experiment with SCTP (native) and SCTP over UDP)
For server-server data transmission, SCTP for servers hosted solely on, Linux/BSD, and TCP for as fall-back
RSVP protocol should be used for server-server connections to initiate a reserved QoS link
Proxy Protocol v2 will be used for ProtOasis Arch proxy-server connections

### Asset Tracking & Licensing
Assets, in this case, are in-game objects which players can create, share, edit, place, destroy, etc.  Assets will require to contain some meta-data: model creator UUID (and name at time of creation), model creation date, asset owner (who does this specific instance of this asset belong to?), date-placed/date-destroyed, player-placed/destroyed (so we know who placed/destroyed a thing) owner-locked (so only the owner may place/break/share/edit the object), expiration, expiry-time (expiration = set time in the future, expiry-time = length of time after being placed - this will make the asset dissolve, cease to exist), Creative (disallows the asset from being used in non-creative zones, and prevents the zone from being set non-creative until all creative assets are removed), Price (custom assets may be sold to players), License (Copyright creator, or Creative Commons license), License-ID (the public half of a key-pair, used to determine if the asset can be sold or not)

None of the Asset meta-data may be changed directly in any way.  Placing, destroying, sharing, etc will update the Asset meta-data as needed.  Assets created in creative zones will not be allowed outside of the creative zone. Player inventories should reflect this.  Custom and Premium assets can be sold for real hard currency as artwork.

Assets can be licensed either with a Copyright, or with the creators choice of Creative Commons license, including CC0, CC-BY, CC-BY-SA, CC-BY-ND, CC-BY-NC, CC-BY-NC-SA and CC-BY-NC-ND. A filter system will need to be implemented to prevent assets from using any other license, and to prevent abuses of licensing / infringement. For Assets with a restrictive license, a keypair needs to be used for the distribution of the assets. The private key will be held by the server, and those assets with a public key can be used only on those servers with the corresponding private key. 

ProtOasis Engineer will be used to assign the license, and generate the keypair.  This is the only time where an asset's meta-data may be changed.  Once the asset has been published, the meta-data may not be changed even with Engineer.  Engineer may only set initial meta-data when the asset is compiled from asset source material.  Engineer will require a ProtOasis account to create new assets, and that account will be used as the creator.  The creator may then choose to keep the asset locally, for manual distribution or to upload it to ProtOasis Expo.  The asset's private and public keys will be uploaded to Expo, and the server owner will then use their server account to download all Private keys and assets through Terminal. Private key stores are encrypted and stored in SQL.  Any asset not distributed through Expo must, as a function of Expo, be CC0 licensed.  Servers will have a generic private key, also stored in this way, with a public key that can be manually applied to assets through Engineer which will allow the custom creation of Copyrighted assets for use solely on that Station. The Server Generic Public key can be shared to community members who wish to create new assets for their Station, in the same way as the server operator would. 

This licensing enforcement allows for the distribution of assets without worry of misuse or infringement, as the asset simply cannot be used if it has a public key without a matching private key. This is handled through in-game store sales, and inventory loadouts, NOT on placement, sharing, etc.

## Game Modes
There will be 5 game modes available. Three of these game modes are controlled by the zone. The last 2 are player-specific and accessible only by those in the Admin Server Group.

**The Zone Modes:**
 * **Play** - can move around, interact, etc but cannot build/destroy/etc - players may take damage, heal, hunger, etc
 * **Build** - same as Play, but may also build/destroy assets
 * **Creative** - has an unlocked creative inventory, does not take damage, hunger, tire, etc. 

**The Admin modes:**
 * **Ghost** - this allows the admin to fly, be invisible, pass through objects, and observe players. There admin will appear completely offline, including in the query list. All servers must abide by this mode. Includes the Admin 'powers'
 * **Admin** - When admins are in Admin mode, they override Zone Modes, being able to fly, destroy assets, build non-creative assets as though they have creative Zone Mode, and will appear online and emphasized in player lists. This will also give the admin their 'powers' above what Staff role has. 
When Ghost and Admin are both toggled off, the admin will abide by Zone Mode and Zone Trust, essentially making the admin a normal player, so they can enjoy the game too! Admins will always retain certain permissions, such as ban, mute, kick, etc.  Luxury permissions such as teleporting to players will be disabled. 

## Core Concept
The goal of ProtOasis is to allow players to build their own little worlds with the freedoms of creation, the barriers of their zone, and the goals of creating their own games within the game, creating groups of players to share in adventures, and to battle to the death. 

There will be players, bosses, pets, docile and aggressive animals, foods, liquids, contraptions, stationary objects, moving objects, terrains, flora, constucts, buildings, passive/filler NPCs and responsive NPCs, vehicles for ground, subterranian, sub aquatic, above sea, air and space. There will be environments from freezing cold to hellish hot, with terrains from land, sea, air, space, etherial planes, underground - in all conditions from near liquid atmospheres to death valley dry land. There will be technological themes from pre-bronze age to super futuristic.  There will be high-tech, low-tech, wizardly magic and witchcraft, mythical powers (think norse gods), holy/unholy powers (Paladins and Necromacers, for example),  From hard science, to hard science-fiction, to whimsical fantasy and mythological stories.  

Player avatars will appear humanoid, will be able to choose their skin color from 512 colors and grays. The base avatar will have white undershirt and underwear (male, female and neuter models will all have the same undershirt and underwear) There will be 8 heights (8", 20", 36" 48" 56" 68" 74" and 96"),  8 Builds (-3, -2, -1, 0,+1, +2, +3, +4), 3 Shapes (un-fit, fit, athletic) several hair and beard styles. Most characteristics of the avatars can be changed to some degree. 

Players will be able to create skins, which they can use to change the appearance of their avatar, minus the shirt and underwear.  There will be clothing assets, which will cover over every aspect of the avatar. 

The player will have access to head, ears (one each), shoulders, back, left and right arm, shirt, 10 ring slots, gauntlets, belt, pants, left and right leg, ankles, and shoe slots for equipment. Items in some slots will consume other slots as well, such as gloves being put in the gauntlet slot disallowing all but one ring on each finger.  These items can be decorative or functional, providing protections, enchantments, etc. 

Server operators will be able to add modules which can change the base game mechanics, such as the shape of avatars being 4 legged beasts instead of humanoid.  Or add multiple avatar models. 

There will be weapons, tools, armor, casual clothing, and other equipment which can act as aesthetic or functional gear.  Assets can also be functional, structural or decorative.  There will be a base set of these assets, along with terrains, buildings, etc for a vanilla spin-up experience for new server operators. 

There will be the concept of character sheets, where players can define their characters to have backgrounds, races, etc.  This can be fleshed out later however.   

As for wearable assets, there should be additional meta-data, stating it's size, weight, etc.  Too heavy, small, or large items cannot be equipped by some characters, whilst others will be encumbered.  This can also affect the wearable's life span, as too small of a shirt can be stretched, worn-out and eventually destroyed by too large of characters.  More on this later as well. 
