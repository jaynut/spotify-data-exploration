# Enhance your Playlists with Machine Learning: Spotify Automatic Playlist Continuation

This is the repository of the group project [Enhance your Playlists with Machine Learning: Spotify Automatic Playlist Continuation](https://medium.com/@enjui.chang/enhance-your-playlists-with-machine-learning-spotify-automatic-playlist-continuation-2aae2c926e77). The four articles in the series is linked below:

**Part I**: [Extracting song data from Spotify’s API in Python](https://cameronwwatts.medium.com/extracting-song-data-from-the-spotify-api-using-python-b1e79388d50)

**Part II**: EDA and Clustering

**Part III**: [Building a Song Recommendation System with Spotify](https://medium.com/@enjui.chang/part-iii-building-a-song-recommendation-system-with-spotify-cf76b52705e7)

**Part IV**: [Deploying a Spotify Recommendation Model with Flask](https://medium.com/@yaremko.nazar/deploying-a-spotify-recommendation-model-with-flask-20007b76a20f)

The code for all four articles is in this repository.

## Introduction

The goal of this project is to recommend songs for a given playlist. This project starts from data collection all the way to model deployment to ensure you have a working model to showcase.

## How to use

To clone the repository:
```sh
git clone https://github.com/enjuichang/PracticalDataScience-ENCA.git
```

## Process

The following image is the flow chart of the project:

<img width="810" alt="Screen Shot 2021-12-18 at 12 02 45 AM" src="https://user-images.githubusercontent.com/55577469/146573138-09798463-c9fe-45b9-adc3-f95556e30564.png">

### Data extraction

Here are a couple of things you should know before starting the project.

#### Spotfiy API Acquisition
If you haven’t used an API before, the use of various keys for authentication, and the sending of requests can prove to be a bit daunting. The first thing we’ll look at is getting keys to use. For this, we need a [Spotify for developers] (https://developer.spotify.com/) account. This is the same as a Spotify account, and doesn’t require Spotify Premium. From here, go to the dashboard and “create an app”. Now, we can access a public and private key, needed to use the API.

#### Spotify Credentials Storage and Access

Now that we have an app, we can get a client ID and a client secret for this app. Both of these will be required to authenticate with the Spotify web API for our application, and can be thought of as a kind of username and password for the application. It is best practice not to share either of these, but especially don’t share the client secret key. To prevent this, we can keep it in a separate file, which, if you’re using Git for version control, should be Gitignored.

Spotify credentials should be stored the in the a `secret.txt` file with the first line as the **credential id** and the second line as the **secret key**:

<img width="293" alt="Screen Shot 2021-12-18 at 12 10 03 AM" src="https://user-images.githubusercontent.com/55577469/146574104-804def73-54ec-449a-931c-86372d3a07a6.png">

To access this credentials, please use the following code:

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
