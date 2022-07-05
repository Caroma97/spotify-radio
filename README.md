# Spotify Radio

Stream your Spotify collection

## Link Collection

[Docker Hub](https://hub.docker.com/r/caroma/spotify-radio/)  
[Github](https://github.com/Caroma97/spotify-radio)  
[Github Compare](https://github.com/comigor/spotify-radio/compare/master...Caroma97:spotify-radio:master)
[Spotifyd Config](https://github.com/Spotifyd/spotifyd) 
[Darkice Config](https://gist.github.com/exiva/1255496)  
[Icecast2 Config](https://icecast.org/docs/icecast-2.4.0/config-file.html)  

### Alexa

[Mein Radio Skill](https://meinradio.esp8266-server.de)  
[Amazon Skill Link](https://www.amazon.de/Michael-Dworkin-Mein-Radio/dp/B08QVB4P26/?tag=esp8266radio-21&linkCode=ez)  
[Radio Config](https://meinradio.esp8266-server.de/senderliste/)  


## Compose File Template

```
services:
  spotify-radio:
    #image: borges/spotify-radio
    image: caroma/spotify-radio:latest
    container_name: spotify-radio
    hostname: spotify-radio
    ports:
      - "20300:20300"
    environment:
      SPOTIFY_USERNAME: $USERNAME
      SPOTIFY_PASSWORD: $PASS
    volumes:
      - /spotify-radio/container/custom_boot.sh:/home/user/custom_boot.sh:ro
      #- /spotify-radio/container/spotifyd:/bin/spotifyd:ro
      - /spotify-radio/container/spotifyd.conf:/home/user/.config/spotifyd/spotifyd.conf:ro
      #- /spotify-radio/container/darkice.cfg:/home/user/darkice.cfg:ro
      #- /spotify-radio/container/icecast.xml:/etc/icecast2/icecast.xml:ro
```

## What?

A docker image that creates a new device on Spotify (called "Daemon"), which streams what you're listening to Icecast.

It uses the excellent [spotifyd](https://github.com/Spotifyd/spotifyd).

## How?

```
docker run --name spotify -p 20300:20300 -e "SPOTIFY_USERNAME=$USERNAME" -e "SPOTIFY_PASSWORD=$PASS" borges/spotify-radio
```

Then connect to the stream via `http://my.ip.address:20300/spotify`.

## Why?

Spotify has an electron app that consumes gigabytes of RAM. I miss the time I used around 100MB to do the same.

So I've been looking for alternatives. Spotifyd came to the rescue but every time my computer went to sleep I had to restart it and reconnect to my Daemon device on Spotify. So I've come up with the idea of dockerize it and let it live on my server, and I chose Icecast to stream it to my machine.

## Disclaimer

[spotifyd](https://github.com/Spotifyd/spotifyd) binary was compiled via `cargo build --release --features pulseaudio_backend` and was included here to accelerate building time. You should compile it by yourself.

## Legal
Spotify Radio in licensed under MIT.

SPOTIFY is a trademark of Spotify AB.
