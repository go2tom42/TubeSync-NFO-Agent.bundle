# YouTube to Plex pipeline

Requirements:

[TubeSync](https://github.com/meeb/tubesync) - [https://github.com/meeb/tubesync](https://github.com/meeb/tubesync)  
[TubeSync NFO Agent](https://github.com/go2tom42/TubeSync-NFO-Agent.bundle) - [https://github.com/go2tom42/TubeSync-NFO-Agent.bundle](https://github.com/go2tom42/TubeSync-NFO-Agent.bundle)  
[Plex](https://www.plex.tv/) - [https://www.plex.tv/](https://www.plex.tv/)

I run everything through Docker using **[caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy)** for ssl, this is my config for [TubeSync](https://github.com/meeb/tubesync), drop the **network** &amp; **labels** sections if you run a different proxy-

      tubesync:
        image: ghcr.io/meeb/tubesync:v0.11.0
        container_name: tubesync
        restart: unless-stopped
        ports:
          - 4848:4848
        volumes:
          - /docker/configs/tubesync/models.py:/app/sync/models.py #NOT Required
          - /docker/configs/tubesync/settings.py:/app/tubesync/settings.py #NOT Required
          - /docker/configs/tubesync:/config
          - /nas/Videos/YouTube:/downloads/video
        environment:
          - TUBESYNC_WORKERS=8
          - TZ=Europe/London
          - PUID=1000
          - PGID=1000
        networks:
          - caddy
        labels:
          caddy: tubesync.XXXXX.pw
          caddy.reverse_proxy: "{{upstreams 4848}}"
          caddy.tls: "internal"


The two "NOT Required" entries are because I wanted larger thumbnails and to change the default settings for new video sources

For the models.py file I changed. Currently lines 237-300 in hash 5bf53b3 (current as of 3-14-22)

    prefer_60fps = models.BooleanField(
        _('prefer 60fps'),
        default=False,
        help_text=_('Where possible, prefer 60fps media for this source')
    )
    prefer_hdr = models.BooleanField(
        _('prefer hdr'),
        default=True,
        help_text=_('Where possible, prefer HDR media for this source')
    )
    fallback = models.CharField(
        _('fallback'),
        max_length=1,
        db_index=True,
        choices=FALLBACK_CHOICES,
        default=FALLBACK_NEXT_BEST,
        help_text=_('What do do when media in your source resolution and codecs is not available')
    )
    copy_thumbnails = models.BooleanField(
        _('copy thumbnails'),
        default=True,
        help_text=_('Copy thumbnails with the media, these may be detected and used by some media servers')
    )
    write_nfo = models.BooleanField(
        _('write nfo'),
        default=True,
        help_text=_('Write an NFO file in XML with the media info, these may be detected and used by some media servers')
    )

For settings.py I changed. Currently lines 148-149 in hash 5bf53b3 (current as of 3-14-22)

    
    MEDIA_THUMBNAIL_WIDTH = 1280                 # Width in pixels to resize thumbnails to
    MEDIA_THUMBNAIL_HEIGHT = 720                # Height in pixels to resize thumbnails to


AND Currently line167 in hash 5bf53b3 (current as of 3-14-22)

    MEDIA_FORMATSTR_DEFAULT = '{source_full} - {yyyy_mm_dd} - [{key}] - {resolution}.{ext}'


But like I said not required. Make sure you enable `Local Media Assets (TV)` in the plex server agents settings for [TubeSync NFO Agent](https://github.com/go2tom42/TubeSync-NFO-Agent.bundle)

[![agents-settings-plex.png](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex1.png)](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex1.png)

My Plex library looks like

[![plex1.png](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex2.png)](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex2.png)  
[![plex2.png](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex3.png)](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex3.png)  
[![plex3.png](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex4.png)](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex4.png)

A "Source" in [TubeSync](https://github.com/meeb/tubesync) looks like using Playlist URL https://www.youtube.com/playlist?list=PL0-LSnSBNInfrr7MDRuXmfk1LJ\_KjvaXt  
[![plex4.png](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex5.png)](https://raw.githubusercontent.com/go2tom42/TubeSync-NFO-Agent.bundle/main/docs/plex5.png)

.
