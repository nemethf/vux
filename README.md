# vux(1) - a rating-based, random ogg and mp3 player

0.4.9, November 04, 2004

```
vux [\|-bcdghjklnquvAFPRSVYZ\|] [\|-a file\|] [\|-e pattern\|] [\|-f value\|] [\|-i option\|] [\|-m option\|] [\|-o value\|] [\|-p file\|] [\|-r value\|] [\|-s file\|] [\|-t value\|] [\|-w option\|] [\|-x option\|] [\|-y file\|] [\|-z file\|] [\|-B value\|] [\|-C value\|] [\|-D value\|] [\|-G value\|] [\|-I value\|] [\|-M value\|] [\|-N value\|] [\|-O device\|] [\|-T value\|] [\|-U value\|] [\|-W option\|] [\|-X value\|]

```

<a name="description"></a>

# Description


**vux** is a command-line ogg and mp3 utility that plays songs
according to a rating system designed to keep track of user listening
habits.  By default, **vux** will give each song a percent chance of
playing, based on where it lies on a bell curve of all ratings.  Songs
are picked at random, then played if they pass the percent chance.
Ratings increase when you play complete songs and decrease when you skip
them.  The ratings scorelist is plain text.  By default, a separate
agelist is maintained to prevent repeats based on time since last
played.  See the **MECHANICS** section for details.

<a name="options"></a>

# Options

When using **vux** for the first time, you must first run vux -x
generate to generate an initial scorelist from a playlist.  See the
**-x generate** option for the playlist format.  In most cases,
**find ~/music -type f &gt; ~/.vux/playlist** will work.  Use -p
_playlist_ to override the default of **~/.vux/playlist**.

* **-a **_file_  
  Use _file_ as the agelist instead of the default.
* **-b**  
  Disable repeat updates when songs are played completely, when using
  **-x weed** or when using **vuxctl next prev** or **replay**.
* **-c**  
  Disable rating checks.
* **-d**  
  Disable rating updates when songs are played completely, when using
  **-x weed**.
* **-e **_pattern_  
  Only play songs matching _pattern_.  Mean and standard deviation are
  still calculated based on all ratings, but only those matching
  _pattern_ are considered for playing.  Globbing follows the rules
  for zsh filename generation; man zshexpn(1) for details.  For example:
  **vux -e '*Artist/Album*'**.  The entire pathname from the scorelist
  is used for matching.
* **-f **_value_  
  Only used with **-x force**.  Force ratings to _value_.
* **-g**  
  Print ratings from **-x ratings** and **-R** in xgraph-friendly
  format.  This also works with xplot.  For example: vux -xr -g |
  xgraph would display a graph where x is rating and y is count.  This
  option implies **-q** and disables **-V**.
* **-h**  
  Print usage.
* **-i **_option_  
  Select increase method for ratings.  Default _option_ is
  **conservative**.

**c|conservative**
Increase less as the current rating gets closer to the maximum score.

* **a|accelerated**  
  Increase more as the current rating gets closer to the middle rating.
* **i|inverse**  
  Increase less as the current rating gets closer to the middle rating.

* **-j**  
  Disable repeat checks.
* **-k**  
  Disable repeat updates when songs are skipped.
* **-l**  
  Disable rating updates when songs are skipped.
* **-m **_option_  
  Select decrease method for ratings.  Default _option_ is
  **conservative**.

**c|conservative**
Decrease less as the current rating gets closer to the minimum score.

* **a|accelerated**  
  Decrease more as the current rating gets closer to the middle rating.
* **i|inverse**  
  Decrease less as the current rating gets closer to the middle rating.

* **-n**  
  Disable ogg and mp3 players.  Useful for testing or batch rating with
  **-Ve\fp pattern**.
* **-o **_value_  
  Only used with **-w middle**.  Use _value_ as the end multiplier
  instead of the default.
* **-p **_file_  
  Use _file_ as playlist instead of the default.  Only useful with
  **-x generate**, **-x merge** and **-x weed**.
* **-q**  
  Minimize vux-generated output.
* **-r **_value_  
  Only used with **-w middle**.  Use _value_ as the middle
  multiplier instead of the default.
* **-s **_file_  
  Use _file_ as the scorelist instead of the default.
* **-t **_value_  
  Add _value_ to the percent chance to play songs that are above or
  equal to the mean.  When used with **-W thresh**, add _value_ to
  threshold.  Value may be positive or negative and may use vux variables
  and formulas.  For example, all of the following are valid: vux -t
  10 ; **vux -t -std\_dev** ; **vux -t '-0.5*std\_dev'**.
* **-u**  
  Check for repeat before checking rating.  Otherwise, ratings are checked
  first.
* **-v**  
  Show version and exit.
* **-w **_option_  
  Select rating method.  Default _option_ is **bell**.

**b|bell**
Use the bell curve.

* **t|thresh**  
  Use thresholds.
* **m|middle**  
  Use distance from fixed middle.

* **-x **_option_  
  Select action for vux to perform.  Default _option_ is **play**.

**p|play**
Play music.

* **g|generate**  
  Generate a new scorelist from playlist and exit.  Playlist must have one
  pathname (the full path to each file, including the filename) per line.
  Spaces, symbols, etc. in the pathname are allowed as the
  line will be double-quoted in the scorelist.  Each song will be given
  the default rating and scorelist will be backed-up and overwritten.
  This is only useful when generating a new scorelist.
* **m|merge**  
  Similar to **generate**, but keeps the existing scores, adding only
  new songs from playlist and giving them the default rating.
* **w|weed**  
  Remove all songs from the scorelist and agelist/countlist that do not
  appear in the playlist and exit.  **-W** determines whether agelist or
  countlist is updated.  **-d** and **-b** disable pruning from the
  scorelist and agelist/countlist respectively.
* **r|ratings**  
  Show rating count, mean, standard deviation and other statistics,
  depending on the rating method used, then exit.
* **f|force**  
  Force ratings of songs selected with **-e** _pattern_.  Without
  additional arguments, this option will cause all ratings of selected
  songs to be increased once using the current increase method.  With
  **-F**, all ratings of selected songs will be decreased once using
  the current decrease method.  With **-f** _value_, all ratings of
  selected songs will be replaced with _value_.

* **-y **_file_  
  Use _file_ as the missing file log instead of the default.
* **-z **_file_  
  Only used with **-W count**.  Use _file_ as the countlist instead
  of the default.
* **-A**  
  Only used with **-W age**.  Disable saving the agelist.  Otherwise,
  **vux** saves the agelist after every 30 songs played and on SIGINT.
* **-B **_value_  
  Only used with **-w middle**.  Use bend factor _value_ instead of
  the default.
* **-C **_value_  
  Only used with **-w thresh**.  Use random reprieve factor _value_
  instead of the default.
* **-D **_value_  
  Use decrease factor _value_ instead of the default.
* **-F**  
  Behave as if songs were skipped when using **-n** or **-x force**.
  The default is to behave as if songs were played completely.
* **-G **_value_  
  Only used with **-W age**.  Use _value_ method to prevent runaway
  age checking in case many songs are below the minimum age.  When
  searching for a song to play, _value_ represents the number of times
  an age check will fail before **vux** will ignore age.  Also valid
  are: _total_ or _t_ for total number of songs, _sqrt_ or
  _s_ for square root of the total number of songs, or _-1_ for no
  method.
* **-I **_value_  
  Use increase factor _value_ instead of the default.
* **-M **_value_  
  Only used with **-W age**.  Use minimum age _value_ instead of the
  default.  A valid value is a number followed by **d** for days,
  **h** for hours, **m** for minutes or **s** for seconds.  For
  example, **7d**, **3h**, **20m**, and **45s** are valid.  The
  status line will display the time since plast played in the time units
  specified here.
* **-N **_value_  
  Only used with **-W count**.  Use count _value_ instead of the
  default.  This is the number of times a song will fail to be chosen
  after being chosen successfully.
* **-O **_device_  
  Use _device_ as the sound device checked before running a player.
  Use **/dev/null** as the device to disable checking.  Default is
  **/dev/dsp**.
* **-P**  
  Only useful with **-W age**.  Songs with no age value will be played
  without a rating check.  This option implies **-u**.
* **-R**  
  Show rating count, mean, standard deviation and other statistics,
  depending on the rating method used.  This is the same as -x
  ratings, but is displayed after any other processing.
* **-S**  
  Disable saving the scorelist.  Otherwise, **vux** saves the scorelist
  after every 30 songs played and on SIGINT.
* **-T **_value_  
  Subtract _value_ from percent chance to play songs that are below
  the mean.  With **-w thresh**, add _value_ to reprieve threshold.
  See **-t**.
* **-U **_value_  
  Use default rating _value_ instead of the default.  This is only
  useful with **-x generate** and **-x merge**.
* **-V**  
  Add a "-v" to cp and rm.
* **-W **_option_  
  Select repeat-checking method.  Default _option_ is **age**.

**a|age**
Test repeats by age.

* **c|count**  
  Test repeats by counting down.

* **-X **_value_  
  Use maximum score _value_ instead of the default.
* **-Y**  
  Disable using the missing log.  Otherwise, songs that are not readable
  (unless beginning with http) will be appended to the missing log.
* **-Z**  
  Only used with **-W count**.  Disable saving the countlist.
  Otherwise, **vux** saves the countlist after every 30 songs played and
  on SIGINT.

<a name="control"></a>

# Control

Control of **vux** is handled by signals, usually through shell or
window manager key bindings such as:

  kill -HUP \`cat ~/.vux/vux.pid\`  
  kill -INT \`cat ~/.vux/vux.pid\`

For a wider range of control, the **vuxctl** program is available.
See **vuxctl(1)** for details.

<a name="signals"></a>

### Signals


* **HUP vux**  
  skip current song and lower its rating
* **INT vux**  
  exit vux, end current song without changing its rating and save current
  scorelist
* **INT player**  
  skip current song but increase the rating (only works when player exits
  0 after receiving **SIGINT** — defaults for player are ogg123 and
  mpg321, which do)

<a name="display"></a>

# Display

When playing songs, **vux** displays the following:

* **decision line**  
  While vux is searching for a song to play at random, the following
  characters are printed:
* **x**  
  Only used with **-w thresh**.  Chosen song was below the reprieve
  threshold.
* **.**  
  Chosen song failed its percent chance to be played.  With -w
  thresh, chosen song was below the threshold, above the reprieve
  threshold and failed the random reprieve chance.
* **!**  
  Only used with **-w thresh**.  Chosen song was below the threshold,
  above the reprieve threshold and passed the random reprieve chance.
  This song will be played unless the **!** is followed by a **:**.
* **+**  
  Chosen song succeeded in its percent chance.  With **-w thresh**,
  chosen song was above the threshold.  This song will be played unless
  the **+** is followed by a **:**.
* **:**  
  Chosen song failed the repeat test.
* **~**  
  Only used with **-W age**.  Chosen song did not meet the minimum age,
  but the age bypass was reached and the song will be played anyway.
* **-**  
  Only used with **-P**.  Chosen song has no age value, the rating
  check will be skipped and the song will be played.
* **status line with -w bell and -W age**  
  current rating/percent chance/mean/standard deviation/age
* **status line with -w thresh and -W age**  
  current rating/threshold/mean/reprieve threshold/age
* **status line with -w bell and -W count**  
  current rating/percent chance/mean/standard deviation
* **status line with -w thresh and -W count**  
  current rating/threshold/mean/reprieve threshold
* **path to song file**  
  Path to song file up to the last forward slash.
* **filename**  
  Filename of song.
* **player output**  
  Normal player output.
* **new rating**  
  If rating updates are enabled, the new rating is displayed after the
  song ends.
* Output from **vuxctl** commands are displayed as they occur.  

<a name="mechanics"></a>

# Mechanics

**vux** uses simple algorithms to decide what song to play and to
determine how much to change the rating.  Two things decide what songs
will be played: the rating method and the repeat-avoidance method.  By
default, **vux** uses the bell curve rating method and the age
repeat-avoidance method.

<a name="choosing-a-song-by-rating-with-w-bell"></a>

### Choosing a Song by Rating with \-w bell

Upon choosing a song at random, **vux** calculates the mean and
standard deviation for all ratings in the scorelist.  **vux** then
applies a simplified bell curve model to the song's rating, to determine
its percent chance of playing.  As the rating approaches +3 standard
deviations from the mean, the chance of playing approaches 100%.  As the
rating approaches -3 standard deviations from the mean, the chance of
playing aproaches 0%.  If the song fails its percent chance, the song is
not played and another is picked at random and the process repeats.

<a name="choosing-a-song-by-rating-with-w-thresh"></a>

### Choosing a Song by Rating with \-w thresh

**vux** uses 2 floating thresholds, the _threshold_ and the reprieve
threshold:

         threshold = ( mean + standard deviation )  
reprieve threshold = ( mean - standard deviation )

These values are calculated before choosing a song and divide the
scorelist into three sections:

* **rating &gt;= threshold**  
  Always play if picked at random.
* **rating &lt; threshold and &gt;= reprieve threshold**  
  1 in 10 (default) chance of playing if picked at random.
* **rating &lt; reprieve threshold**  
  Never play if picked at random.

If the song does not qualify to be played, another song is picked at
random and the process repeats.

<a name="choosing-a-song-by-rating-with-w-middle"></a>

### Choosing a Song by Rating with \-w middle

This is the simplest rating method.  Rating probabilities are determined
by these variables (defaults shown):

      max_score=100  
      min_score=0  
      bend_factor=10  
      end_mult=1.5  
      middle_mult=1.1  

**vux** takes the song's rating and uses that as its probability of
playing (if **max\_score** and **min\_score** are not 100 and 0
respectively, **vux** first extrapolates it into a 0-100 scale.)  This
probability is modifed by multiplying it either by **end\_mult** or
**middle\_mult**.  **middle\_mult** is used for ratings that are
between middle rating + **bend\_factor** and middle rating -
**bend\_factor**.

<a name="avoiding-repeats-with-w-age"></a>

### Avoiding repeats with \-W age

After a song is played, **vux** associates a timestamp with the song.
When the song is chosen again, the timestamp is subtracted from the
current time and compared to **minimum age**.  If the result is not
greater that **minimum age**, the song is not played.

<a name="avoiding-repeats-with-w-count"></a>

### Avoiding repeats with \-W count

After a song is played, **vux** associates a number with the song.
This number is set with **-N** and defaults to 10.  When the song is
chosen again, the number is decremented and the song is not played.
Once the number reaches 0, the song will be available to play again.

<a name="changing-ratings"></a>

### Changing Ratings

Rating changes are controlled by these variables (defaults shown):

      max_score=100  
      min_score=0  
decrease_factor=10  
increase_factor=10

When a song is skipped, its rating changes according to this formula:

**decrease = integer of(current_rating / - decrease_factor)**

With **-m accelerated**, the following formula is used instead:

**x = current_rating - min_score**  
**y = max_score - current_rating**  
**decrease = integer of(maximum of(1,(minimum of(x,y)/2)) * -1**

With **-m inverse**, the following formula is used instead:

**x = max_score - min_score**  
**y = absolute value of ( current_rating - x )**  
**decrease = integer of( y / decrease_factor ) - 1 ) * -1**

When a song is played all the way through (or the player is sent a
SIGINT), its rating changes according to this formula:

**increase = integer of((max_score - current_rating) / increase_factor)**

With **-m accelerated**, the following formula is used instead:

**x = current_rating - min_score**  
**y = max_score - current_rating**  
**increase = integer of(maximum of(1,(minimum of(x,y)/2))**

With **-m inverse**, the following formula is used instead:

**x = max_score - min_score**  
**y = absolute value of ( current_rating - x )**  
**increase = integer of( y / increase_factor ) + 1 )**


<a name="diagnostics"></a>

# Diagnostics

**vux** will exit 0 if there are no errors.  Otherwise, the following
exit codes are used:

* **1**  
  Usage error.
* **2**  
  Unable to create vux working directory.
* **3**  
  Scorelist lockfile already exists.
* **4**  
  Unable to load scorelist.
* **5**  
  No matching songs found with **-e**.
* **6**  
  Unable to open socket.

<a name="examples"></a>

# Examples


* **vux -x generate**  
  Generate a new scorelist from $HOME/.vux/playlist and exit.
* **vux -x ratings**  
  Show song ratings and exit.
* **vux -x merge -RU 60 -p $HOME/.vux/newsongs**  
  Merge $HOME/.vux/newsongs with $HOME/.vux/scorelist, adding new songs
  with a rating of 60, show ratings after merging and exit.
* **vux -cdS**  
  Play songs, but do not choose songs based on rating, update ratings or
  save the scorelist.
* **vux -jbA**  
  Play songs, but do not avoid repeats based on age, update ages or save
  the agelist.
* **vux -W count -jbC**  
  Play songs, but do not avoid repeats based on count, update counts or
  save the countlist.
* **vux -M 4d -G -1**  
  Do not repeat songs for 4 days, display ages in days and never bypass
  age checking.
* **vux -W count -N 20**  
  Do not repeat songs for 20 attempts.
* **vux -k**  
  Do not update age on skipped songs.
* **vux -t 10 -T 20**  
  Add 10% to the percent chance to play songs above the mean and subtract
  20% from the percent chance to play songs below the mean.
* **vux -w thresh -t 10 -T -20**  
  Use the threshold rating method, add 10 to threshold and subtract 20
  from reprieve threshold.
* **vux -w thresh -T std_dev**  
  Use threshold rating method, add current standard deviation value of all
  ratings to reprieve threshold, limiting song choice to mean and above.
* **vux -dbnSAV**  
  Do not change ratings or ages, do not play music and do not save
  scorelist or agelist.  This will show what vux would do without
  actually playing anything.
* **vux -e '*Artist/Album*'**  
  Only consider songs matching *Artist/Album*.
* **vux -e '*Artist/(Album1|Album2)*'**  
  Only consider songs matching *Artist/Album1* or *Artist/Album2*.
* **vux -cje '(#i)*whale*'**  
  Play songs with whale in the pathname, ignoring case, rating and age.
* **vux -cjnVFe '(#i)*purse*'**  
  Choose songs with purse in the pathname, ignoring case, rating and age.
  Do not actually play the songs, but display the song name and lower its
  rating.  This will continue choosing songs from the selection and
  lowering ratings indefinitely.
* **vux -x force -e '*Artist*'**  
  Increase the rating of each song matching *Artist* once, using the
  conservative increase method, then exit.
* **vux -x force -e '*Artist*' -F**  
  Decrease the rating of each song matching *Artist* once, using the
  conservative decrease method, then exit.
* **vux -x force -e '*Artist*' -f 100**  
  Set the rating of each song matching *Artist* to 100, then exit.

<a name="bugs"></a>

# Bugs

Use the Debian Bug Tracking System for reporting bugs and making
suggestions.

<a name="see-also"></a>

# See Also

**zshexpn(1), vuxctl(1)**

<a name="files"></a>

# Files


* /etc/vuxrc  
  system configuration file
* $HOME/.vux/vuxrc  
  user configuration file
* $HOME/.vux/playlist  
  default playlist to generate, merge or weed scorefiles from — this is
  never modified by vux
* $HOME/.vux/scorelist  
  default scorelist
* $HOME/.vux/agelist  
  default agelist
* $HOME/.vux/countlist  
  default countlist
* $HOME/.vux/missing  
  default missing file log
* $HOME/.vux/*.bak  
  default backup file; made before saving
* $HOME/.vux/*.lock  
  default lockfile preventing a file save or load while saving
* $HOME/.vux/vux.pid  
  file containing PID of any vux process using -x play
* $HOME/.vux/ctl  
  vux control socket

<a name="author"></a>

# Author

This manual page was written by Brian Nelson &lt;[bnelson@bloodclot.net](mailto:bnelson@bloodclot.net)&gt;,
for the Debian GNU/Linux system (but may be used by others).
