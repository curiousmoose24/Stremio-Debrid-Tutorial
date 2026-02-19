# Stremio-Debrid-Tutorial
This is my preferred Stremio setup with Debrid integration, consisting only of what I think is essential for the best experience. There are more detailed tutorials linked at the bottom of this document that including features like AI search functionality. If that interests you, feel free to refer to them.
## Debrid Services & Stremio
A subscription to a debrid service like [Real-Debrid](https://real-debrid.com) and/or [Torbox](https://torbox.app) allows Stremio to access a wealth of cached content, meaning it can be served instantly without buffering or downloading. Real-Debrid has more content than Torbox, but Torbox is generally more user friendly. I would recommend starting with Real-Debrid. Use any payment method you like.

Both Stremio and RD require an email account. I would recommend making a separate email using a service like [Proton Mail](https://proton.me/) or [TutaMail](https://tuta.com/) since will be making accounts on various different sites. This isn't strictly necessary, I just like having a separate email for everything Stremio related.

Once you have created a Stremio account and purchased a debrid subscription, continue to the next section.
## AIOStreams
Stremio's power lies in its addons. Addons are the link between the content in a debrid service and a frontend like Stremio. [This](https://stremio-addons.net/) is the most well maintained, up-to-date repository of addons. AIOStreams simplifies and streamlines the addon process. Use the [Midnight Instance](https://aiostreamsfortheweebsstable.midnightignite.me) or check the [uptime log of all instances](https://status.dinsden.top/status/stremio-addons) (some publicly hosted instances disallow the use of Torrentio, which is why I recommend using the Midnight instance). I'll go through each AIOStreams category individually.
### Services
Enable your debrid services of choice, paste API keys (RD API [Link](https://real-debrid.com/apitoken)) (Torbox API [Link](https://torbox.app/settings?section=account))
### Addons
**AIOStreams** integrates lots of addons itself, but some need to be added manually.
- **AIOMetadata** improves search functionality and content filtering. Use the [midnight instance](https://aiometadatafortheweebs.midnightignite.me/) or check the [uptime log of all instances](https://status.dinsden.top/status/stremio-addons). I'll go through each tab of it:
	- Presets: Select based on your preferences (Movies & Shows Only if you never watch anime)
	- General: Exclude adult content
	- Integrations: Paste TMDB and TVDB API keys (accounts required, follow the included links), Poster Rating Provider: RPDB, Enable Proxy Ratings Posters
	- Metadata Providers: Movies: TMDB, Series: TVDB
	- Art Providers: Movies default, Series TVDB
	- Content Filters: adjust to your liking
	- Disable Catalogues and Search functions
	- Configuration: Check API Keys Statuses & click Save Configuration
	- Create a password, record UUID and password somewhere, and then copy the Install URL
	- Go back into AIOStreams -> Addons -> Marketplace -> Miscellaneous -> Custom
	- Paste the AIOMetadata Install URL into the Manifest URL box, naming it AIOMetadata, & install
- **Streaming catalogues** sorts shows and movies by streaming services (Netflix, Prime, Disney+)
	- Reorder or enable/disable catalogues to your liking
- [**Submaker**](https://submaker.elfhosted.com/) fetches high quality subtitles
	- Enable Just Fetch Subtitles
	- Obtain as many API keys for as many services as you like, with a minimum of just OpenSubtitles
	- Select English as your language
	- Check "Include Season Pack Subtitles"
	- Save your configuration, copy the install URL
	- Go back into AIOStreams -> Addons -> Marketplace -> Miscellaneous -> Custom
	- Paste the Install URL into the Manifest URL box, naming it Submaker, & install
- **Scrapers** grab links for your debrid services. They ask the debrid service if something is cached, and initiate the caching process if it isn't. Install the following scrapers, and adjust the Timeout (ms) to between 5,000 and 10,000 from the default 15,000.
	- Torrentio 
	- StremThru Torz, paste this url in the url bar: (https://stremthrufortheweebs.midnightignite.me)
	- Comet, paste this url in the url bar: (https://comet.feels.legal)
	- Meteor isn't in the AIOStreams marketplace as of writing. Follow the custom addon instructions as previously described, pasting this url in the url bar: (https://meteorfortheweebs.midnightignite.me)
	- Knaben
	- Jackettio
	- TorrentGalaxy
	- TorrentsDB
- Go to "Installed" and at the bottom of the page, go to Addon Fetching Strategy. Select Dynamic and paste this text:
```
((count(cached(regexMatched(resolution(language(quality(totalStreams, 'Bluray REMUX', 'Bluray', 'WEB-DL') 'English') '2160p')))) >= 3 and (count(cached(regexMatched(resolution(totalStreams, '2160p')))) >= 5 or count(cached(regexMatched(resolution(totalStreams, '1080p')))) >= 5) and count(cached(regexMatched(quality(totalStreams, 'Bluray REMUX', 'Bluray', 'WEB-DL', 'WEBRip')))) >= 5) or count(cached(totalStreams)) >= 3 and totalTimeTaken > 7000) or totalTimeTaken > 10000
```
### Filters
- Quality: Exclude CAM, TS, TC, SCR
- Encode: Prefer AV1, HEVC, AVC, Unknown (in that order)
- Visual tag: Exclude 3D and all HDR tags, depending on your TV
- Audio tag: Prefer Atmos, DD plus, DD (in that order)
- Language: Prefer English, Original, Dual audio, multi, dubbed, unknown (in that order)
- Matching mode: exact, enable strict year matching, enable season/episode matching. Request types for all sections: movie, series, anime
- Keywords: exclude 'r00', 'iso' 
- Deduplicator Enabled, per addon
### Sorting
Sort by cached
### Formatting
The formatting section affects how stream names are served to you. 

This formatting uses emojis to convey information, the most important one being the⚡️lightning bolt, signifying a cached link ready to stream immediately. The ⏳ hourglass signifies a link that still needs to be cached. Uncached links can be selected, but will return a message like "This stream is being downloaded, please wait". The link is being downloaded by the debrid service, and may be ready in as little as a few minutes. It is very rare that there are no cached links to content.

Select Custom, paste my custom formatting as seen below:

Name Template:
```
{stream.resolution::=2160p["⚜️ 4K UHD "||""]}{stream.resolution::=1080p["⚜️ 1080P "||""]}{stream.resolution::=1440p["⚜️ 1440P "||""]}{stream.resolution::=720p["HD 720P "||""]}{stream.resolution::=576p["Low Quality "||""]}{stream.resolution::=480p["Low Quality "||""]}{stream.resolution::=360p["Low Quality "||""]}{stream.resolution::=240p["Low Quality "||""]}{stream.resolution::=144p["Low Quality "||""]}{stream.resolution::exists[""||"⚜️ Unknown "]}

❖ {addon.name} {service.shortName::exists["[{service.shortName}"||""]}{service.cached::istrue["⚡️]"||""]}{service.cached::isfalse["⏳]"||""]}
```

Description Template:
```
{stream.quality::exists["🎥 {stream.quality} "||""]}{stream.encode::exists[" 🎞️ {stream.encode}"||""]}{stream.encode::exists["{tools.newLine}"||""]}{stream.visualTags::exists["💠 {stream.visualTags::join(' | ')} "||""]}{stream.regexMatched::exists[" 🏷️ {stream.regexMatched}"||""]}

{stream.audioTags::exists["🔊 {stream.audioTags::join(' | ')} "||""]}{stream.audioChannels::exists[" 🎧 {stream.audioChannels::join(' | ')} "||""]}

{stream.size::>0["📦 {stream.size::bytes}"||""]}{stream.languages::exists[" 🗣 {stream.uLanguageEmojis::join(' / ')}"||""]}

{stream.type::=usenet["🌐 Usenet "||""]}{stream.type::=debrid["🌐 Debrid "||""]}{stream.type::=p2p["🌐 P2P "||""]}{stream.filename::~AMZN["🔹 Amazon "||""]}{stream.filename::~NF["🔻 Netflix "||""]}{stream.filename::~DSNP["🌌 Disney+ "||""]}{stream.filename::~HMAX["🔘 HBO Max "||""]}{stream.filename::~APTV["♠ Apple TV+ "||""]}{stream.filename::~Hulu["🟢 Hulu "||""]}{stream.filename::~PRME["🔹 Prime Video "||""]}{stream.filename::~PMTP["🔶 Paramount+ "||""]}{stream.filename::~PCKK["🟠 Peacock "||""]}{stream.filename::~CRTC["🔶 Crunchyroll "||""]}{stream.filename::~ANPX["💎 Anime Plex "||""]}{stream.filename::~STZ["🔶 Starz "||""]}{stream.filename::~DSCV["🌍 Discovery+ "||""]}{stream.filename::~OSN["🔴 OSN+ "||""]}{stream.proxied::istrue[" 🕵️‍♂️ Proxy "||""]}{stream.library::istrue[" 🔰 Library"||""]}

{stream.title::exists["🎬 {stream.title::title} "||"🎬 {stream.filename}{tools.newLine} "]}{stream.year::exists["({stream.year}) "||""]}{stream.seasonEpisode::exists["{stream.seasonEpisode::join(' • ')}"||""]}{stream.filename::~Extended[" [Extended Cut] "||""]}{stream.filename::~Theatrical[" [Theatrical Cut] "||""]}{stream.filename::~Director[" [Director's Cut] "||""]}{stream.filename::~Redux[" [Redux] "||""]}{stream.title::exists[""||"{tools.removeLine}"]}

{stream.quality::exists["{tools.removeLine}"||"📑 {stream.filename}"]}{stream.title::exists[""||"{tools.removeLine}"]}
```
### Miscellaneous:
- Enable Precache next episode
- Enable auto play with method matching file and autoplay attributes of resolution and quality
- Enable cache and play
- Enable hide errors
### Save & Install
- Save your configuration, recording the UUID and password you set
- Install into Stremio, either through the web app or the desktop app
- Go to https://cinebye.dinsden.top/ 
	- Paste Stremio credentials
	- download existing stremio backup
	- Remove Cinemata search, catalogs, metadata
	- Sync addons

All done! Stremio is now ready to use.
# More Detailed Guides I Referenced
## [Viren070 Guide](https://guides.viren070.me/stremio/addons/aiostreams/setup)
## [Duck Guide](https://duckkota.gitlab.io/guides/)
## Reddit Guides
https://old.reddit.com/r/StremioAddons/comments/1qvn9rk/my_stremio_perfect_setup_for_me_at_least_sharing/
https://old.reddit.com/r/StremioAddons/comments/1q2du2j/as_promised_my_full_stremio_build_guide_using/
https://old.reddit.com/r/StremioAddons/comments/1mzqfgn/best_aio_metadata_setup_stable_clean_spoilerfree/
