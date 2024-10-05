# Preface
These are essentially the same as my CSGO configs, they had to be modified to work on CS2 but everything is functionally identical, with the exception of certain commands that had to be removed because they are not currently in the game. I will try to keep an eye out for updates in the future but you can always [submit an issue](https://github.com/abandonedpools/hlae-cfg-cs2/issues) if I miss anything or you have a good idea for a feature to add.

# Overview
This is my config for recording CS2 demos with HLAE. I made it to be quick and easy to use, with plenty of customization options. It records at a high framerate and blends the frames to create a motion blur effect (similar to sony vegas resample or after effects frame blending).

The "raw" version of the cfg is functionally the same but doesn't apply motion blur to the rendered clips.

# Requirements
- [HLAE (with FFMPEG)](https://github.com/advancedfx/advancedfx/releases)

 ***Use the installer and tick the checkbox that says "reinstall FFMPEG"**

# How to use
## Basic instructions

1. Download adam.cfg from the [releases](https://github.com/abandonedpools/hlae-cfg-cs2/releases) tab.
2. Place adam.cfg in your config folder (`...Counter-Strike Global Offensive\game\csgo\cfg`).
2. [Launch CS2 with HLAE.](https://github.com/advancedfx/advancedfx/wiki/AfxHookSource2#launching-counter-strike-2)
2. Open the demo and type "exec adam" in console.
3. Go to the round/tick you want to record and press <kbd>o</kbd> on your keyboard to start recording (you can change this bind in the cfg file).
4. Once you are done, press <kbd>o</kbd> again to stop recording.

By default the recordings will be saved in `...Counter-Strike Global Offensive\game\bin\win64` but you can change this in the config [as shown here](#optional-commands).

If you do not see an mp4 file or encounter other issues please refer to the [bottom of this page](#troubleshooting).

## Simple command shortcuts
Aliases you can type in console to quickly execute/toggle commands.

| Alias | Command(s) | Description |
| --- | --- | --- |
| localplayer | mirv_deathmsg localplayer xTrace | (toggle) Highlight kills of the player currently being spectated. **(may not work with pov demos)** |
| block | mirv_deathmsg filter add attackerMatch=!xTrace victimMatch=!xTrace block=1 lastRule=1 | (toggle) Block kills that are not from the player you are currently spectating **(may not work with pov demos)** |
| lifetime | mirv_deathmsg lifetimeMod 15 | (toggle) Make highlighted kills stay in the killfeed, only enable this RIGHT before your clip or kills from previous rounds may also show up in the killfeed, and disable it before moving to the next clip |
| clearmsg | mirv_deathmsg filter clear; mirv_deathmsg lifetimeMod default; mirv_deathmsg localplayer default | Clear all  killfeed filters (last 3 commands) |
| id | mirv_deathmsg help players | Show player XUID's in console |
| radar | toggles cl_drawhud_force_radar (0/-1) | Toggle the radar off/on |
| hud | toggles cl_draw_only_deathnotices (0/1) | Toggle the hud off/on (minus the crosshair and killfeed) |
| commands | n/a | Print all the commands shown above in the console |

## Customizing blur/video settings
| command | default value | description |
| --- | --- | --- |
| mirv_streams record fps | 1080 | This is how many frames per second the game will run at during recording, higher will make the blur look smoother and **will not** make the file size bigger, but it will take longer. 900 and up should look good |
| mirv_streams settings add ffmpeg mp4 "-c:v libx264 -pix_fmt yuv420p -preset ultrafast **-crf 9** {QUOTE}{AFX_STREAM_PATH}\video.mp4{QUOTE}" | 9 | These are the FFPEG encoding settings which I do not recommend changing unless you know what you are doing. The only exception is crf, which is the bitrate of your video. Lower = better quality but larger file size. ([what is crf?](https://trac.ffmpeg.org/wiki/Encode/H.264#crf)) |
| mirv_streams settings edit blur exposure | 0.7 | This is how much blur the recording will have, this setting is good if you render your final video at 24-30 fps but you might want more blur for lower framerate fragmovies |
| mirv_streams settings edit blur fps | 60 | This is the fps that your recording will be. 60 is good enough for most fragmovies but you can make it higher if you need to. This setting will also affect how much blur your video has. (e.g. 60 fps 1 exposure = 120 fps 2 exposure = 240 fps 4 exposure... etc.) |

### Note:
The last two settings do not appear in the "raw" version of the cfg since it does not have a sampler, the output fps is instead determined by the `mirv_streams record fps` command which is defaulted to 60. Note that higher framerates may require you to adjust the bitrate (crf value, lower = better), leading to larger files.

## Optional commands
These commands are already in the cfg file but are disabled by default, to enable them remove the "//" in front of them.

| Command(s) | Description |
| --- | --- |
| cl_show_observer_crosshair 0 | show your current crosshair rather than the one you had during the match |
| mp_display_kill_assists false | Removes assists from killfeed |
| mirv_viewmodel enabled 1; mirv_viewmodel set 2 0 -2 68 0 | overrides viewmodel, settings work as follows: `<X> <Y> <Z> <FOV> <0\|1>` with the last value being 0 for right-handed and 1 for left-handed |
| mirv_streams record name "[path]" | Custom recording path (might record faster if you export to an SSD) |

# Troubleshooting
## No video file
If you do not see a video file, make sure you ticked the "reinstall FFMPEG" checkbox when installing HLAE. you must use the installer, not the portable zip.

## Error when launching HLAE
If you get an error when launching HLAE, it means it's either out of date or a game update broke certain features. Note that if you just click OK the game *might* still launch and you *might* still be able to record demos if none of the features used here are affected.

## Other issues
If your issue isn't listed here you can create an issue in the [issues](https://github.com/abandonedpools/hlae-cfg-cs2/issues) tab, or politely ask for help in the [HLAE discord](https://discord.gg/NGp8qhN).
