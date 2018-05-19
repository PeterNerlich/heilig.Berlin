# HBP Post Production Workflow

## Tools used
- Ardour >=5 (DAW)
- Calf Studio Gear LV2 plugins
- [Auphonic](https://auphonic.com/) Multitrack Processing

## Raw Recording Format
- mixed track for alignment reference (WAV/32 bit float, 44100 Hz)
- Mono: "Stephan" (WAV/32 bit float, 44100 Hz)
- Stereo (joint mono tracks: "Dietmar", "Jana") (WAV Signed 16 bit PCM, 44100 Hz)

--------

## Pre-adjustment
- create new 44100 Hz Ardour session
- import as 4 distinct tracks
- sync tracks (use sync point in middle to average out drift)
- mute sync track
- set start and end marker
- if running on auphonic free budget (meaning multitrack processing limited to 20min)
	- splitting podcast in ~3 slightly (> 2s) overlapping sections < 20min by setting range markers named `proc[1|2|3]`
	- stem export for each section by applying the following configuration:
		- File format
			- Format:	BWAV 24bit Export: BWF, 24-bit, Session rate
			- Location
				- Build filename(s) from these components:
					- [No Name]
					- Label:	
					- Timespan name:	[enabled]
					- [No Date]
					- [No Time]
		- Timespan
			- session [disabled]
			- each `procX` [enabled] only in its pass
		- Channels
			- Master	[disabled]
			- sync	[disabled]
			- stephan	[enabled]
			- dietmar	[enabled]
			- jana	[enabled]
- else
	- stem export by applying the following configuration:
		- File format
			- Format:	BWAV 24bit Export: BWF, 24-bit, Session rate
			- Location
				- Build filename(s) from these components:
					- [No Name]
					- Label:	
					- Timespan name:	[enabled]
					- [No Date]
					- [No Time]
		- Timespan
			- session [enabled]
		- Channels
			- Master	[disabled]
			- sync	[disabled]
			- stephan	[enabled]
			- dietmar	[enabled]
			- jana	[enabled]

## adjustment
- check for an existing multitrack preset with the following settings:
	- Preset name
		- name:	raw episode
	- Audio Tracks and Algorithms
		- Identifier:	Stephan
			- Filtering:	[enabled]
			- Noise and Hum Reduction:	[enabled], [Auto]
			- Fore/Background:	[Foreground Track]
		- Identifier:	Dietmar
			- Filtering:	[enabled]
			- Noise and Hum Reduction:	[enabled], [Auto]
			- Fore/Background:	[Foreground Track]
		- Identifier:	Jana
			- Filtering:	[enabled]
			- Noise and Hum Reduction:	[enabled], [Auto]
			- Fore/Background:	[Foreground Track]
	- Intro and Outro
		- (leave empty)
	- Basic Metadata
		- Title:	0XX
		- Cover Image:	(leave empty)
		- Artist:	heilig.Berlin
		- Album:	heilig.Berlin Podcast
		- Track:	0XX
	- Extended Metadata
		- Subtitle:	(leave empty)
		- Genre:	(leave empty)
		- Year:	2018
		- Append Chapters to Summary:	[disabled]
		- Summary:	(leave empty)
		- Publisher:	(leave empty)
		- URL:	https://heilig.berlin/
		- License URL:	(leave empty)
		- Tags:	(leave empty)
	- Output Files
		- Format: [Individual Tracks]
			- Filename Suffix:	(leave empty)
			- Ending:	[wav.zip]
		- Format: [Audio Processing Statistics]
			- Filename Suffix:	(leave empty)
			- Ending:	[txt]
		- Output File Basename:	0XX
	- Speech Recognition BETA
		- (leave empty)
	- Publishing / External Services
		- (leave empty)
	- Master Audio Algorithms
		- Adaptive Leveler:	[enabled]
		- Adaptive Noise Gate:	[enabled]
		- Crossgate:	[enabled]
		- Loudness Target:	[-16 LUFS (Podcasts and Mobile)]

- if running on auphonic free budget (meaning multitrack processing limited to 20min), check for an existing multitrack preset with the same as above, differing in the following settings:
	- Preset name
		- name:	raw episode proc
	- Basic Metadata
		- Title:	0XX procX
	- Output Files
		- Output File Basename:	0XX procX

- use the template to create a new production (or multiple when split into sections), inserting the right files and replacing 0XX with the episode count (and procX with proc[1|2|..], if applicable)
- saving (uploading) and then proofreading is suggested, especially checking file names coherence with proc replacements
- launch production(s) and wait for download


## Cutting
- save the existing ardour session as a copy named "auphonic out" and keep working on the current version
- delete all tracks
	- import the downloaded files from auphonic as new tracks and rename to [stephan|dietmar|jana] according to their content (keeping that order is advised)

*[TODO]*


## Post-Adjustment
- set the track panning (L/R): 60/40 (stephan), 46/54 (dietmar), 40/60 (jana)
- create new plugins "Gate" (LADSPA, by Steve Harris) and "Calf Equalizer 8 Band" (LV2, by Calf Studio Gear) for one track, set up as follows (as in generic controls) and copy+paste to the remaining tracks
	- Strip plugin rack
		- Gate
			- LF key filter (Hz):		30.870
			- HF key filter (Hz):		21609.000
			- Threshold (dB):			-60.000
			- Attack (ms):			5.000
			- Hold (ms):				100.000
			- Decay (ms):				100.000
			- Range (dB):				-90.000
			- Output select:			[gate]
		- Fader
			- -0.0 dB
		- Calf Equalizer 8 Band (where every automation setting should be [manual])

Name				|	Value
--------------------|-----------------------
Bypass				|	[disabled]
Individual Filters	|	[enabled]
**Analyzer Active**	|	**[enabled]**
Input Gain			|	1.000
Output Gain			|	1.000
HP Active			|	[ ]
HP Freq				|	30.00 Hz
HP Mode				|	[24dB/oct]
HP Q				|	* 0.71
LP Active			|	[ ]
LP Freq				|	18000.00 H
LP Mode				|	[24dB/oct]
LP Q				|	* 0.71
LS Active			|	[ ]
Level L				|	1.000
Freq L				|	100.00 Hz
LS Q				|	* 0.71
**HS Active**			|	**[ON]**
**Level H**			|	**2.000**
**Freq H**			|	**3600.00 Hz**
HS Q				|	* 0.71
F1 Active			|	[ ]
Level 1				|	1.000
Freq 1				|	100.00 Hz
Q 1					|	* 1.00
F2 Active			|	[ ]
Level 2				|	1.000
Freq 2				|	500.00 Hz
Q 2					|	* 1.00
**F3 Active**			|	**[ON]**
**Level 3**			|	**1.300**
Freq 3				|	2000 Hz
Q 3					|	* 1.00
**F4 Active**			|	**[ON]**
**Level 4**			|	**1.300**
Freq 4				|	5000.00 Hz
Q 4					|	* 1.00
Zoom				|	0.250
Analyzer Mode		|	[Difference]

## Export+Publishing
- export whole session as `session.ogg` by applying the following configuration:
	- File format
		- Format:	Ogg/Vorbis: normalize peak, Ogg Vorbis, Session rate
		- Location
			- Build filename(s) from these components:
				- [No Name]
				- Label:	(leave empty)
				- Timespan name:	[enabled]
				- [No Date]
				- [No Time]
	- Timespan
		- session [enabled]
	- Channels
		- Master	[enabled]
		- each [stephan|dietmar|jana]	[disabled]
		- Cannels:	2
		- Split to mono files:	[disabled]
- convert to MP3, average bit rate 128 kbps joint stereo (e.g. using Audacity)
	- set tags:
		- Artist Name:	heilig.Berlin
		- Track Title:	0XX
		- Album Title:	heilig.Berlin Podcast
		- Track Number:	0XX
		- Year:			2018
- upload to server, rename to hbp0XX.mp3
