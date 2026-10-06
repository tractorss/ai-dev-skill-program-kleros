# 2. One rediscovery

## What I'd dropped, and why

A Japanese learning game built around the Cure Dolly script — top-down pixel
graphics, learning the grammar by exploring a valley.

Two things stopped it. The graphics were bad, and designing the game felt like a
large amount of work before anything was playable. Concretely: I'd have had to
spend real time planning levels and scenes, and generating the assets.
Building the map itself is what I like, but it's the most time consuming part. Agents can create a scene given the assets, but it doesn't fit the 32x32 gridlines. (We can't just use the normal png from the agent, because it needs the JSON exported map too, from Tiled).

## What I actually attempted

A trimmed version: move around the map, interact with an NPC, take a quiz. Other
features as stretch goals.

- **Bounded change:** make the map, let the player move around it.
- **Observable success:** the player walks the map and cannot walk over the
  collision shapes.
- **Time budget:** 1 hour.

## Still to answer

- What actually became possible?

  - While not really tested in this project, I worked on Okiya during last days and the sfx was something that Claude did purely by itself and it gave great results. For the game design, research and panning was handed to Fable (high) and in two turns (after feedback) the result was more than satisfactory. Purely because I had Opus to do a a deep research on game design and physcology around it first and that was fed to Fable when it acted as an advisor and critique on the game.
  - Another thing that's possible is the workflows, I can construct mine or have the agent first do a research and construct one based on the task size and complexity.

- What still needed my judgment?

  - The game design for this project and Okiya both needed my judgement. On the logic side, the only instruction I passed was to keep core logic separate from UI. Since this makes it easy to plug new UI/design on the existing system.

- What I'll try next:

  - I would try to have Fable/Opus consume the assets and make multi level maps. I did try to use this in Okiya game, but the usage of assets was limited to the UI design and not the map. While there was good usage of assets for ambient things like particles, small animations, start-screen etc.,
  - Other thing that I'm working on is _Foreman_ which acts as my personal manager and helps me give an overview of my work, my notes, my ongoing sessions. More on this in the report or later in the week once it's fully built and tested.
