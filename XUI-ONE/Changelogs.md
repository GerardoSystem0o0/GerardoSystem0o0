RELEASES XUI one

Update 1.5.13 Released
[CRITICAL] Patched an exploit in the System API that could allow for remote read and write if leveraged correctly.
[Core] Reverted EPG system to previous MySQL based system to fix a bug where EPG wasn't being retained.
[Core] Fixed EPG API calls and images

v1.5.12 R1 - 31/10/2021:
[Core] Rewritten Activity Logs, Error Logs, Panel Logs, Stream Logs and Client Logs to stream all data through Redis to the main Server to be written to the database from there. Sending large amounts of data over MySQL from a load balancer with high ping can cause slowdowns and MySQL service instability. Redis can handle this better.
[Core] Added global Rate Limiting to NGINX configuration. GZIP compression on main is now optional and both can be set via Edit Server.
[Core] Modified Migration script to accept NXT databases, as well as only show data ready for migration and removing blanks.
[Core] Added indexes to lines table to speed up page loads when having hundreds of thousands of lines.
[Core] Fixed Loopback when using SSL on servers.
[Core] Reduced error logs for streams failing to start if they are loopback, as this isn't an error if the feeder source is down.
[Core] Fixed Redis ended connections not writing to activity logs.
[Core] Fixed datatype of parent_id on servers. This will allow Global Proxy to work as expected now.
[Core] Removed legacy rate limiting code (limit.ini).
[Admin] Fixed Start, Stop and Restart streams button in Multi-Select mode on Servers page.
[Admin] Fixed Proxy Tools button not opening after changing page.
[Admin] Fixed pagination on Show Failures modal.
[Admin] Fixed success message when flushing blocked IP's.
[Admin] Added extra checks to Update License functionality to avoid invalid writes.

v1.5.12 - 30/10/2021:
[Core] Rewritten Proxy system to allow proxies to cover mulitple servers. Load balancing system rewritten to handle this. Reinstall your proxies as the old system won't work anymore. More tools and functionality is coming soon, this is just the first pass.
[Core] Modified PHP Loopback to skip content-type checks to ensure better compatibility. Should work for those who it didn't work with before.
[Core] Modified load balancing and offline video behaviour to optimise it.
[Core] Added max_requests variable to PHP-FPM instances to automatically restart PHP-FPM when a certain amount of requests has been handled.
[Core] Removed umount commands from updates and load balancer installations so streams and tmp directory don't get unmounted and lose their contents.
[Core] AppArmor is automatically disabled when Update Binaries is run, or load balancers are installed.
[Core] Added a background bouquet check to ensure that deleted streams are removed from bouquets.
[Core] Modified transcoding profiles to add a comma after scaling, may fix some errors people have.
[Core] Modified URL in Player API to match the domain name or proxy used.
[Core] Removed redundant checks when killing PID's.
[Core] Removed redundant checks from startup script.
[Admin] Added Multiselect Actions to Servers, Proxies, Users, Lines, MAG's, Enigmas, Streams, Channels, Stations, Movies, Episodes and Series. Hold SHIFT and click individual rows to select them, the action bar appears in the header to run actions on multiple items at once.
[Admin] Holding CTRL when clicking a link should now open it in a new tab, if the function is compatible.
[Admin] Removed Subreseller Setup and Subresellers page, you can add subreseller groups via Edit Group page.
[Admin] Fixed dashboard graph Users count not being unique when server are grouped.
[Admin] Fixed editing servers in Mass Edit Channels, Episodes and Stations.
[Admin] Added MariaDB version check to the dashboard. If an outdated version is found you will be notified.
[Admin] Modified tooltip behaviour to stop duplicates layering over eachother sometimes.
[Admin] When clicking a server in Mass Edit, the server edit checkbox will be selected automatically.
[Admin] Added Update Server button to Server Tools to force an update.
[Admin] Show Failures button now shows per server when streams aren't grouped.
[Reseller] If multiple subreseller groups are applied to a group, the reseller can now select which group they want the user they are creating to be in.
[Reseller] Modified Extend log to show as Edit if no package is applied and 0 credits are used.

v1.5.11 R3 - 26/10/2021:
[Core] Fixed issue with Stream Monitor not checking playlists to see if they've been changed or not.
[Core] Rewritten LLOD & Loopback segment parsers to be purely PHP as the C mpegts parser was using too much CPU running in the background.
[Core] Fixed network statistics being doubled.
[Core] Fixed Timeshift not displaying in apps due to player API call being broken.
[Admin] Fixed Users not showing in Graph Stats.
[Admin] Reworked Settings layout so that items are in more relevant places.

v1.5.11 R1 - 24/10/2021:
[Core] New loopback CLI replaces FFMPEG passthrough with a PHP based alternative. Fast, lightweight, uses 50% as much CPU and isn't susceptible to audio desync. You can turn this off in Settings.
[Core] Completely rewritten EPG system, no longer stores programme information in MySQL. Should be faster and use less CPU usage when generating EPG. All system calls to read EPG have been rewritten to read from cache instead.
[Core] Fixed caching of streams in Stream Providers, they will now be deleted if provider no longer provides stream.
[Core] Fixed issue with 100% CPU usage on TV Archive if the folder get's deleted while it's running.
[Core] Written a lightweight mpeg-ts information parser in C to replace calls to ffprobe, allowing HLS playlists to be generated with accurate segment durations without the added load of calling ffprobe. For LLOD and new Loopback system.
[Core] Added sync byte detection to LLOD, if the stream desyncs it will scan for the next mpeg-ts sync byte and resynchronise the stream without just exiting (if possible).
[Core] Added a segment check to Stream Monitor, if the segment is unchanged for 5 minutes it will exit as it's likely that FFMPEG has encountered an error that means the segments are no longer splitting and the current segment is growing exponentially.
[Core] Added a shuffle statement when load balancing so if two servers have the same number of connections it will pick a random one rather than the first one. Useful as the number of connections update every 10 seconds by default and it will keep connecting new users to the first one during that time otherwise.
[Core] Modified API redirect function to allow legacy apps to redirect users to the AES encrypted token stream URL's instead of the legacy format.
[Core] Removed .ts extension if Cloudflare toggle is enabled.
[Core] Forced loopback live streaming code to exit immediately if stream stops running. This is to avoid FFMPEG issues with desync of mpeg-ts segments when a stream restarts.
[Core] If Stream Offline video is disabled and cache is enabled, a check to see if the stream is online is performed before any other code is run. This will save a lot of CPU cycles if an offline stream is hit repeatedly.
[Core] Enabled GZIP compression on main nginx configuration to reduce network output on main server.
[Core] Written a network monitor in C to continuously run in the background and provide XUI with more accurate statistics.
[Core] Fixed time offset in EPG, was accidentally using seconds instead of minutes.
[Core] Fixed an issue where a warning would display on the dashboard for outdated tables even when they're not outdated.
[Admin] Added Restart Running Streams for entire server to Servers page actions.
[Admin] Modified Network Stats to group queries for speed.
[Admin] Fixed number of active connections for Proxies in Server View page.
[Admin] Modified feedback API for Proxies to get more accurate network stats.
[Admin] Removed Proxies from dashboard until the new Global Proxy system is completed.
[Admin] Modified information box layout on Dashboard to be 3x2 instead of 6x1 for clearer statistics.
[Admin] Fixed issue where the action box would disappear due to refreshing information on Stream View page.
[Admin] Updated Process Monitor to display average CPU usage and total running time rather than just CPU time.
[Admin] Fixed Connections not showing correctly on Stats Graph.
[Web Player] Rewritten EPG functions to use new caching system.
[Ministra] Rewritten EPG functions to use new caching system.

v1.5.10 R1 - 19/10/2021:
[Core] Removed aes.php CLI script that handles the encryption of HLS segments. This is now done when a client connects and encrypted segments are stored for other connecting users.
[Core] Modified Authorised Flood protection to slow down genuine clients who connect over and over again. You can modify the flood limit settings to suit your needs. The default is 30 requests per 10 seconds, the user will then be slowed down by 1 second on any subsequent requests for the next 10 seconds.
[Admin] Modified Transcoding Profiles to better handle software decoding. Useful for MPEG2 streams on GPU's with no mpeg2_cuvid support.

v1.5.10 - 18/10/2021:
[Core] Reworked Probe code to increase speed and reduce errors due to timeouts. XUI will request prebuffer when probing sources to get more data quicker for compatible streams.
[Core] Removed mariadb update code from update script due to issues with the update stalling.
[Core] Update now sets permissions on license to ensure any existing permission issues are resolved.
[Admin] Added Add Source as Backup and Update Existing options to Import Streams (via M3U). Will add a source as a backup if the title exists, or will update stream options if the stream source exists.
[Admin] Refined import via M3U code to increase speed.
[Admin] Added Proxy servers and server type subheader to dashboard.
[Admin] Added server order page to reorder servers on dashboard and server list when adding content.
[Admin] Added LLOD v3 check when probing sources, will display a tick if compatible.
[Admin] Added threshold percentages to settings to change server status thresholds before showing as an error.
[Admin] Clicking an active server in the server tree when adding content will remove it from active servers.
[Admin] Added message box to Group add page to display a formatted message to resellers when they login.
[Admin] Added filter to Live Connections to filter by Lines only, MAG's only, Restreamers or Trials etc. Non-redis only.
[Admin] Added Expiring Soon filter to Lines page (within 14 days).
[Reseller] Added Header Stats to dashboard. Shows amount of connections and online users.
[Migrate] Added custom migration options and database scanning to help identify any issues.

v1.5.9 - 17/10/2021:
[Core] Added keyframe analysis to timeshift, should allow playback without requiring a hardware decoder or alternative decoding method in apps such as Smarters IPTV and others that show no video sometimes on timeshift streams. Restart your timeshift channels to begin keyframe analysis.
[Core] Modified on-demand startup code to exit quicker if start fails, would previously wait too long.
[Core] Reworked stats to give a better CPU average and more accurate current CPU usage. Previous method used an average over a longer period of time so it looked like CPU was still high after a spike.
[Core] Modified keyframe analysis code to ensure PAT headers are available when a keyframe is found, if it isn't it waits for the next keyframe otherwise you'll get no video. LLOD v3
[Core] Added basic Stream Errors to LLOD v3 to diagnose why streams aren't working.
[Core] Removed passthrough FFMPEG support from LLOD v3, it only works with MPEG-TS sources so use LLOD v2 for HLS.
[Core] Added movie extension to encrypted playlists.
[Admin] Reworked Mass Delete, Mass Edit Streams, Channels, Movies, Episodes, Series and Stations to display more information.
[Admin] Added ability to Set, Add or Remove servers in Mass Edit Streams, Channels, Movies and Episodes.
[Admin] Added No Categories and No Servers filter to pages.
[Admin] Placed server name in title of page instead of XUI.

v1.5.8 R2 - 13/10/2021:
[Core] Reverted the NGINX binary back to pre-1.5 version, removed additional changes to NGINX config.
[Core] Modified NGINX configuration of Proxy to ensure the Proxy IP is forwarded to the XUI server. Reinstall proxies to avoid issues with IP forwarding not working or connections not establishing.
[Core] Rewritten parts of LLOD v3 to reduce CPU and memory usage after feedback.
[Core] Modified Flood Protection to avoid false positives and be more efficient. Should be much better now!
[Core] Modified Stream Monitor to allow on-demand streams more time to start before showing failed if a segment has started being written.
[Core] Modified stream authentication bouquet check to ensure new streams are available for existing clients immediately when caching is enabled.
[Admin] Added Sysctl editor to Server edit page. Also included default configurations to revert changes.
[Admin] Added CPU Governor selection to Server edit page. For this to work, either reinstall load balancer or install cpufrequtils separately using "apt install cpufrequtils".
[Admin] Clicking stream failure count now shows a popup allowing you to see restarts and clear failures.
[Admin] Changed Proxy Source to Direct Stream to avoid confusion over what it does.
[Admin] Fixed Mass Edit Streams when attempting to apply LLOD v3.
[Ministra] Fixed an issue where some variables would be encapsulated in quotes when updating the database.

v1.5.8 - 11/10/2021:
[Core] Integrated LLOD v3 - PHP based segmenter written inhouse to replicate FFMPEG functionality. Experimental at the moment but you can select LLOD v3 instead of LLOD v2 and it should be faster and not have any audio loss issues. Works for MPEGTS streams only, anything else you give it will be run through FFMPEG first to output MPEGTS.
[Core] Further rewrites to Live Proxy to improve delivery of prebuffer to new clients. Faster connections for 2nd connection onwards.
[Core] Updated recordings CLI tool to ensure that if at least 10MB of data is written, the file is written to disk in the event of a stream failure or early cancellation.
[Core] Disabled encrypted HLS segments when using on-demand to improve speed.
[Core] Automatic switch to -f segment when using LLOD v2 to ensure streams start faster.
[Core] Rewritten VOD and Timeshift to ensure app reconnections don't deny access due to expiration of token. If a client reconnects it will be allowed, however if the connection is closed by reaching the connection limit or by an admin through the panel the token will be killed too. Much better protection.
[Core] Added token storage to Plex Sync so the cron doesn't have to login at every run, if the token becomes invalid it will log in and store the new token. Should fix issues with Plex API login limits.
[Core] Reduced sleep timers in live streaming from 1 second to 0.1 seconds to ensure faster delivery when waiting for a new segment to be written.
[Core] Plex Sync now checks more places for TMDB ID and rating as for series this can be stored in multiple places in the API return. More accurate results now.
[Core] Recompiled xui.so to add extra checks to ensure config.ini isn't cleared by mistake.
[Core] Added extra checks on various functions to avoid panel error build up.
[Core] Added stack tracing to exception logs so I know specifically where to look when a user submits panel error logs.
[Core] Added Delete Duplicates to CLI tools, when running from Quick Tools the work is done in the background instead and will no longer crash out on big queries.
[Core] Added VOD information such as Plot, Cast, Director, Genre, Release Date, Run Time and Youtube Trailer to the Player API for app development purposes.
[Admin] Added a status check to the dashboard when tables haven't been updated.
[Admin] Fixed issue with input and output using ceil instead of floor. i.e. 1Mbps instead of 0Mbps when under 1024Kbps.
[Admin] Added more checks for direct proxy switch.
[Reseller] Added more checks to ensure direct reports are correct, will avoid SQL errors.
[Web Player] Fixed ordering of series when sort by VOD date added setting is turned on.
[Ministra] Added support for TH100 and TH200 MAG STB's

v1.5.7 R3 - 06/10/2021:
[Core] Added Direct Proxy to Plex Sync - Uses the streaming URL for Plex items without the files having to be on the same server. Can select multiple servers to enable load balancing.
[Core] Fixed an issue with cover images not storing on Plex Sync movies.

v1.5.7 R2 - 06/10/2021:
[Core] Further rewrites to experimental Live Proxy, higher compatibility and lower CPU consumption. New packet handler.
[Core] Fixed URL forwarding on VOD Proxy causing some VOD not to play.
[Core] Modified rate limiting for VOD Proxy.
[Core] Added stream stats for proxied Live streams.
[Core] Modified EPG cron to avoid duplicates, fix issue with writing XML files and increase run time.
[Core] Client connections will be denied if settings cache doesn't yet exist. To mitigate error with decryption keys.

v1.5.7 R1 - 05/10/2021:
[Core] Added packet inspection to Live Proxy to look for the MPEG-TS header and determine where the keyframes would start, allowing the prebuffer to consist of the correct header when delivering to clients. Should see far higher compatibility for clients as players won't have to guess what codecs they're displaying.
[Core] Added HLS as an input method for Live Proxy. Still limited to MPEG-TS output only. HLS uses FFMPEG passthru to change the transport stream to MPEG-TS, and therefore uses more memory and resources.
[Core] Added non-blocking sockets to support higher throughput when using Live Proxy.

v1.5.7 - 04/10/2021:
[Experimental] Added Direct Proxy option for Streams (MPEG-TS only). This will proxy direct streams and passthrough to multiple clients while only using one connection to the original source. No segments are saved to disk and the original URL is not disclosed. Only bandwidth is consumed.
[Experimental] Added Direct Proxy option for Movies and Episodes. This will proxy direct VOD streams and passthrough to a client without storing the VOD or disclosing the original URL. Only bandwidth is consumed.
[Core] Fixed target container including GET arguments when automatically detected.
[Core] Removed redundant license check that was causing some people to not have access to their admin panel.
[Core] Added some expiration checks to VOD and Timeshift.
[Core] Removed VOD from error scanning cron to save CPU cycles.
[Admin] Fixed domain name check not allowing domains with extensions longer than 6 characters.

v1.5.6 R1 - 28/09/2021:
[Core] Added sysctl check to root signals which will ensure the sysctl has been written correctly.
[Core] Fixed issue with EPG cron duplicating entries when saving an EPG API enabled stream.
[Core] Fixed M3U header not showing version number.
[Admin] Added Root Crons check to Dashboard to ensure they're running as expected.
[Web Player] Modified EPG Guide to show stream title and use resize handler for picons.
[Web Player] Fixed filter by category on Movies and Series.

v1.5.6 - 24/09/2021:
[Core] Rewritten NGINX IP handling to mitigate packet loss.
[Core] Fixed GPU transcoding error due to software_decoding option.
[Core] Implemented timeshift support for SmartIPTV users.
[Core] Direct access to streams is now denied if proxy is enabled.
[Core] Fixed usage of TMDB duplicate option in Watch Folder. Enable it to allow duplicate TMDb ID's.
[Core] Downloaded images now have the original URL encrypted in the filename so they can be recovered.
[Admin] Added Restore Lost Images quick tool to redownload images that can be decrypted if missing.
[Admin] Fixed tooltip refreshing causing stacked elements.
[Admin] Fixed dropdown box on Movie view page.
[Admin] Added Recordings page, custom event recording, record button to EPG view and TV Guide.
[Reseller API] Fixed issue allowing non-editable fields to be modified in some circumstances.
[Proxy] Repackaged with updated IP forwarding. Please reinstall your proxies after updating.


Tools
The release comes with a tools script to help you repair an installation you no longer have access to or are having issues with.

To execute: /home/xui/tools [OPTION] [ARGUMENTS]

Here are the options:
rescue - Create a rescue access code for the admin panel.
user - Create a rescue admin user for the admin panel.
mysql - Reauthorise load balancers on MySQL.
database - Restore blank XUI database.
migration [optional: *.sql] - Clear migration database, optionally imports selected SQL file into xui_migrate database.
flush - Flush blocked IP database.
ports - Regenerate ports from SQL table.
access - Regenerate access codes from SQL table.
