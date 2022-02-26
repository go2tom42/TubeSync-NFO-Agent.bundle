# TubeSync-NFO Agent

I started with [Youtube-DL-Agent.bundle](https://github.com/JordyAlkema/Youtube-DL-Agent.bundle), then I butched and hacked it to my will.  Along the way I used [mcinj's](https://github.com/mcinj) [work](https://github.com/mcinj/Youtube-DL-Agent.bundle/tree/tubesync_xml?rgh-link-date=2022-02-01T01%3A24%3A38Z), then added more localmedia support

## What does this agent do?

It uses NFO files from generated from [tubesync](https://github.com/meeb/tubesync)



## Installation

In order to use this Agent you need to use the [Absolute Series Scanner from ZeroQI](https://github.com/ZeroQI/Absolute-Series-Scanner), You can try using it without however plex will think files are copies when they are not.

1. Download this repository ([CLICK HERE](https://github.com/go2tom42/TubeSync-NFO-Agent.bundle/archive/master.zip))
2. Unzip the file but keep the root folder ("TubeSync-NFO-Agent.bundle-master")
3. Rename the folder to "TubeSync-NFO-Agent.bundle"
4. Move the folder into the "Plug-ins" folder in your Plex Media Server installation ([Wait where?](https://support.plex.tv/articles/201106098-how-do-i-find-the-plug-ins-folder/))
5. Create a new (or update an existing) library and select "TV Show"
6. Click on the "Advanced" tab and select
   1. Scanner: Absolute Series Scanner
   2. Agent: TubeSync-NFO

Now you are done. At first Plex will scan for all the files, when this is done the agent will attempt to find the metadata associated with the videos.

## Troubleshooting

This works for my needs and I don't have a clue what I'm doing so if you have any issues you are your own.  Sorry

## Thanks to...

**[ZeroQI](https://github.com/ZeroQI)** - ZeroQI created the original Youtube Agent, the Absolute Series Scanner which this agent also uses and he has a bunch of helpful posts on the Plex forums.  
**[mcinj](https://github.com/mcinj)** - for adding NFO support to [Youtube-DL-Agent.bundle](https://github.com/JordyAlkema/Youtube-DL-Agent.bundle)  
**[meeb](https://github.com/meeb)** - for creating [tubesync](https://github.com/meeb/tubesync)  
