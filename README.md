#### Spotfiy API Acquisition
We're using the spotify The first thing we’ll look at is getting keys to use. For this, we need a [Spotify for developers] (https://developer.spotify.com/) account. This is the same as a Spotify account, and doesn’t require Spotify Premium. From here, go to the dashboard and “create an app”. Now, we can access a public and private key, needed to use the API.  

#### Spotify Credentials Storage and Access

Once you have an app, you will also have a Client ID, and a Client Secret. Both of these will be required to authenticate with the spotify API (alongside a working browser), and can be thought of as a kind of username and password for the application.  Make sure you keep them in a split out file and .gitignore them!

The way I set up spotify credential storage is detailed below in the "Secrets and Setup" section.

### Secrets and Setup:

In order to load data, you need a config file that configures certain environment variables that the spotify API will use.  Here's an example config.py file:

```py
import os
import spotipy
import spotipy.util as util

def init():
    os.environ["PORT_NUMBER"] = "8080"
    os.environ["SPOTIPY_CLIENT_ID"] = 'ID_FROM_SPOTIFY_DEV_PORTAL'
    os.environ["SPOTIPY_CLIENT_SECRET"] = 'SECRET_FROM_SPOTIFY_DEV_PORTAL'
    os.environ["SPOTIPY_REDIRECT_URI"] = 'http://localhost:8080/callback'
    os.environ["SCOPE"] = 'user-library-read'
    os.environ["CACHE"] = '.spotipyoauthcache'
    os.environ["USER"] = "jaynut"


def get_auth():
    
    token = util.prompt_for_user_token(username= os.getenv('USER'),
    scope = os.getenv('SCOPE'), redirect_uri = os.getenv('SPOTIPY_REDIRECT_URI'))

    if token:
        return spotipy.Spotify(auth=token)
    else:
        raise "Failed to authenticate with spotify API!  Please confirm your token details."
```

## Repo Structure
```
│
├── README.md            <- The top-level README, this file!
├── data
│   ├── liked_songs.csv  <- The spotify data!  It's my current liked songs, padded with the spotify api.
│
│
├── notebooks                   <- Jupyter notebooks!
│   ├── load_data               <- Script for generating a liked_songs.csv of your own, to be inspected with another script
│   ├── quick_data_inspection   <- Simple plots/data displays to inspect the csv file, and see if there's any obvious trends
```
