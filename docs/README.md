# YouTube to Plex pipeline

Requirements:

[TubeSync](https://github.com/meeb/tubesync) - [https://github.com/meeb/tubesync](https://github.com/meeb/tubesync)  
[TubeSync NFO Agent](https://github.com/go2tom42/TubeSync-NFO-Agent.bundle) - [https://github.com/go2tom42/TubeSync-NFO-Agent.bundle](https://github.com/go2tom42/TubeSync-NFO-Agent.bundle)  
[Plex](https://www.plex.tv/) - [https://www.plex.tv/](https://www.plex.tv/)

I run everything through Docker using **[caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy)** for ssl, this is my config for [TubeSync](https://github.com/meeb/tubesync), drop the **network** &amp; **labels** sections if you run a different proxy-

```YAML
  tubesync:<br></br>    image: ghcr.io/meeb/tubesync:latest<br></br>    container_name: tubesync<br></br>    restart: unless-stopped<br></br>    ports:<br></br>      - 4848:4848<br></br>    volumes:<br></br>      - /docker/configs/tubesync/models.py:/app/sync/models.py #NOT Required<br></br>      - /docker/configs/tubesync/settings.py:/app/tubesync/settings.py #NOT Required<br></br>      - /docker/configs/tubesync:/config<br></br>      - /nas/Videos/YouTube:/downloads/video<br></br>    environment:<br></br>      - TUBESYNC_WORKERS=8<br></br>      - TZ=Europe/London<br></br>      - PUID=1000<br></br>      - PGID=1000<br></br>    networks:<br></br>      - caddy<br></br>    labels:<br></br>      caddy: tubesync.XXXXX.pw<br></br>      caddy.reverse_proxy: "{{upstreams 4848}}"<br></br>      caddy.tls: "internal"
```

The two "NOT Required" entries are because I wanted larger thumbnails and to change the default settings for new video sources

For the models.py file I changed. Currently lines 237-300 in hash 5bf53b3 (current as of 3-14-22)

```Python
    prefer_60fps = models.BooleanField(<br></br>        _('prefer 60fps'),<br></br>        default=False,<br></br>        help_text=_('Where possible, prefer 60fps media for this source')<br></br>    )<br></br>    prefer_hdr = models.BooleanField(<br></br>        _('prefer hdr'),<br></br>        default=True,<br></br>        help_text=_('Where possible, prefer HDR media for this source')<br></br>    )<br></br>    fallback = models.CharField(<br></br>        _('fallback'),<br></br>        max_length=1,<br></br>        db_index=True,<br></br>        choices=FALLBACK_CHOICES,<br></br>        default=FALLBACK_NEXT_BEST,<br></br>        help_text=_('What do do when media in your source resolution and codecs is not available')<br></br>    )<br></br>    copy_thumbnails = models.BooleanField(<br></br>        _('copy thumbnails'),<br></br>        default=True,<br></br>        help_text=_('Copy thumbnails with the media, these may be detected and used by some media servers')<br></br>    )<br></br>    write_nfo = models.BooleanField(<br></br>        _('write nfo'),<br></br>        default=True,<br></br>        help_text=_('Write an NFO file in XML with the media info, these may be detected and used by some media servers')<br></br>    )<br></br>
```

For settings.py I changed. Currently lines 148-149 in hash 5bf53b3 (current as of 3-14-22)

```Python
MEDIA_THUMBNAIL_WIDTH = 1280                 # Width in pixels to resize thumbnails to<br></br>MEDIA_THUMBNAIL_HEIGHT = 720                # Height in pixels to resize thumbnails to
```

AND Currently line167 in hash 5bf53b3 (current as of 3-14-22)

```Python
MEDIA_FORMATSTR_DEFAULT = '{source_full} - {yyyy_mm_dd} - [{key}] - {resolution}.{ext}'
```

But like I said not required. Make sure you enable `Local Media Assets (TV)` in the plex server agents settings for [TubeSync NFO Agent](https://github.com/go2tom42/TubeSync-NFO-Agent.bundle)

[![agents-settings-plex.png](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/scaled-1680-/u0Qagents-settings-plex.png)](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/u0Qagents-settings-plex.png)

My Plex library looks like

[![plex1.png](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/scaled-1680-/plex1.png)](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/plex1.png)  
[![plex2.png](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/scaled-1680-/plex2.png)](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/plex2.png)  
[![plex3.png](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/scaled-1680-/plex3.png)](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/plex3.png)

A "Source" in [TubeSync](https://github.com/meeb/tubesync) looks like using Playlist URL https://www.youtube.com/playlist?list=PL0-LSnSBNInfrr7MDRuXmfk1LJ\_KjvaXt  
[![plex4.png](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/scaled-1680-/plex4.png)](https://bookstack.tom42.pw/uploads/images/gallery/2022-03/plex4.png)

.
