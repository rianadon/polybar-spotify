# polybar-spotify

This is a module that shows the current song playing and its primary artist on Spotify, with a Spotify-green underline, for people that don't want to set up mpd. If Spotify is not active, nothing is shown. If the song name is longer than `trunclen` characers (default 25), it is truncated and `...` is appended. If the song is truncated and contains a single opening parenthesis, the closing paranethsis is appended as well.

### Dependencies
- Python (2.x or 3.x)
- Python `dbus` module
- Python `spotipy` module (optional)

[![sample screenshot](https://i.imgur.com/kEluTSq.png)](https://i.imgur.com/kEluTSq.png)

### Settings
~~~ ini
[module/spotify]
type = custom/script
interval = 1
format-prefix = " "
format = <label>
exec = python /path/to/spotify/script
format-underline = #1db954
~~~

#### Custom arguments

##### Truncate

The argument "-t" is optional and sets the `trunlen`. It specifies the maximum length of the song name, so that it gets truncated when the specified length is exceeded. Defaults to 25.

Override example:

~~~ ini
exec = python /path/to/spotify/script -t 42
~~~

##### Format

The argument "-f" is optional and sets the format. You can specify how to display the song and the artist's name. Useful if you want to swap the positions.

Override example:

~~~ ini
exec = python /path/to/spotify/script -f '{song} - {artist}'
~~~

This would output "Lone Digger - Caravan Palace" in your polybar, instead of what is shown in the screenshot.

##### Liked songs / songs added to library

If the "-s" and "-u" arguments are used, the script will use the Spotify API to determine whether you have saved the current song to your library (i.e. favorited it). The argument "-s" specifies the text or icon to use if the song has been saved, and the "-u" options speficies text to use if it is not saved.

~~~ini
exec = python /path/to/spotify/script -f '{saved}{artist}: {song}{unsaved}' -s 's: ' -u ' :u'
~~~

This would output "s: Caravan Palace: Lone Digger" if the song were saved, and "Caravan Palace: Lone Digger :u" if the song were not saved. Note that "{song}" and "{unsaved}" need to be specified in the format to control where the saved/unsaved text is placed.

In order to use the Spotify API, you'll need a client ID and client secret from the [development page](https://developer.spotify.com/documentation/general/guides/app-settings/#register-your-app). Those need to be specified in `~/.config/spotify/barconfig.cfg` as such:

~~~ini
[authorization]
username = email@example.com
client_id = ...
client_secret = ...
~~~

These options get passed to the `spotipy` module.
You'll also need to add the redirect uri `http://localhost` to the list of redirect uris. If you would like to use a different url, edit the `saved_tracks.py` script. Nothing is required to be running on that port.

After specifying the config, run `spotify_status` with the `-s` option manually to authenticate the script iwth the Spotify API.
