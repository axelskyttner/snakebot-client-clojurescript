# Cygni Clojurescript Snake Bot Client

### Do you want to be one of the cool kids?

You are now standing at a cross road in your life. You have been tasked with writing the best snake bot ever. There are a number of paths to walk down, a number of hats to pull your rabbit out of. But the important thing to know is that this is the only righteus one.

I present to you a clojurescript template for your snake-bot. This template is the first step on your journey.

LISP (Luxurious Immense Super Programming) will take you out of the tar pit and acend you into a higher being. A being that only speak in parentheses and data. This is the path towards the pearly gates and it will turn you into the Alpha and the Omega of programming. 

When you have walked the path of the righteus you will earn a seat in the high heavens and you will gaze down from your mountain of awesomenes upon the lesser beings of the world.

# Get it running

1. Install leiningen. Follow the instructions at http://leiningen.org/
2. with a command prompt/ terminal traverse to the root folder where the project.clj file is located
3. lein deps
4. lein cljsbuild once
5. node ./server.js

# Structure

- root
  - src
    - cljs_snake_bot
      - snake_examples (In here there are two examples, one random snake and one snake that tries to not hit the walls nor other snakes)
    - constants.cljs (All the message identifiers used)
    - core.cljs (Contains the websocket handling and the game loop. The game loop is just a busy wait loop that continues until the game ends)
    - event.cljs (The reactions to different messages. This piece will update the current game state and also respond to different events with a new message)
    - messagehandler.cljs (Parses the current message and calls the correct event handler)
    - messages.cljs (Message templates)
    - printer.cljs (Console/Terminal printer)
    - settings.cljs (Current game settings and game state)
  - project.clj (Leiningen project file that contains dependencies and stuff)

# What to change

### Settings.cljs

Update the settings of your client either if you want to change what the printer prints, the map used for the games, if you are the host for the game or not, or what hostname/port/gamemode the server is running.

### Events.cljs

Here is where you will include your snake. Update the final line of "on-map-updated" and pass it any valid movement directional string and it will send that to the server. A snippet can be found below:

```clojure
(defn on-map-updated [msg]
  (p/print-map-updated-message msg)
  (swap! s/game-tick inc)
  (msgs/get-move-message @s/player-id @s/game-tick "DOWN")) // Change "DOWN" into something else
```

  I would place my snake in its own namespace. Require that namespace into this file and call some get-move function that returns your next move. An example can be found below
  
- super-snake.cljs
```clojure
  (ns cljs-snake-bot.snake-examples.super-snake
    (:require [cljs.core :as core]))

  (defn get-next-movement [msg] "UP")
```
  
- events.cljs
```clojure
  (ns cljs-snake-bot.events
    (:require [cljs.nodejs :as nodejs]
              [cljs-snake-bot.messages :as msgs]
              [cljs-snake-bot.settings :as s]
              [cljs-snake-bot.printer :as p]
              [cljs-snake-bot.snake-examples.super-snake :as ss])
              
    (defn on-map-updated [msg]
      (p/print-map-updated-message msg)
      (swap! s/game-tick inc)
      (msgs/get-move-message @s/player-id @s/game-tick (ss/get-next-movement msg))
```
And now your snake should start moving upwards instead.

Woho!

### Project.clj

Update this one if you need more NodeJs dependencies or if you need to optimize your solution or something similar.
To see how NodeJs dependencies are used. Look at the core.cljs file.


You have now the ability to be one of the cool kids. Yay you!