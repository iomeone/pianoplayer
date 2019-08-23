# PianoPlayer 2.1
Automatic piano fingering generator. <br />
Find the optimal fingering combination to play a piano score 
and visualize it in 3D with [vtkplotter](https://github.com/marcomusy/vtkplotter)
and [music21](http://web.mit.edu/music21).<br />

## Download and Install:
```bash
pip install --upgrade pianoplayer
```

To visualize the output annotated score install the latest [musescore](https://musescore.org/en/download), or any other renderer of [MusicXML](https://en.wikipedia.org/wiki/MusicXML)
files.

#### Optionally:
To be able open a 3D interactive visualization, install [vtkplotter](https://github.com/marcomusy/vtkplotter) the command line:
```bash
pip install --upgrade vtkplotter

#to enable sound you may need to:
sudo apt install libasound2-dev
pip install simpleaudio
```
Windows 10 user can place this file: [pianoplayer.bat](https://github.com/marcomusy/pianoplayer/blob/master/pianoplayer.bat) ontheir desktop (edit the path to your local anaconda installation).

## CLI Usage: 
Example command line from terminal:<br />
`pianoplayer scores/bach_invention4.xml --verbose -n10 -rvm`<br />
will find the right hand fingering for the first 10 measures, 
pop up a 3D rendering window and invoke *musescore*.

The output is saved as a [MusicXML](https://en.wikipedia.org/wiki/MusicXML)
file with name `output.xml`.<br />

```bash
pianoplayer         # if no argument is given a GUI will pop up (on windows try `python pianoplayer.py`)
# Or
pianoplayer [-h] [-o] [-n] [-s] [-d] [-k] [-rbeam] [-lbeam] [-q] [-m] [-v] [--vtk-speed] 
            [-z] [-l] [-r] [-XXS] [-XS] [-S] [-M] [-L] [-XL] [-XXL]
            filename
# Valid file formats: MusicXML, musescore, midi (.xml, .mscz, .mscx, .mid)
#
# Optional arguments:
#   -h, --help            show this help message and exit
#   -o , --outputfile     Annotated output xml file name
#   -n , --n-measures     [100] Number of score measures to scan
#   -s , --start-measure  Start from measure number [1]
#   -d , --depth          [auto] Depth of combinatorial search, [2-9]
#   -rbeam                [0] Specify Right Hand beam number
#   -lbeam                [1] Specify Left Hand beam number
#   --verbose             Switch on verbosity
#   -m, --musescore       Open output in musescore after processing
#   -b, --below-beam      Show fingering numbers below beam line
#   -v, --with-vtk        Play 3D scene after processing
#   -z, --sound-off       Disable sound
#   -l, --left-only       Fingering for left hand only
#   -r, --right-only      Fingering for right hand only
#   -x, --hand-stretch    Enable hand stretching
#   -XXS, --hand-size-XXS Set hand size to XXS
#   -XS, --hand-size-XS   Set hand size to XS
#   -S, --hand-size-S     Set hand size to S
#   -M, --hand-size-M     Set hand size to M
#   -L, --hand-size-L     Set hand size to L
#   -XL, --hand-size-XL   Set hand size to XL
#   -XXL, --hand-size-XXL Set hand size to XXL
```

### GUI Usage:<br />
Just type `pianoplayer` in a terminal
(or double click the [pianoplayer.bat](https://github.com/marcomusy/pianoplayer/blob/master/pianoplayer.bat) file),
then:

![newgui](https://user-images.githubusercontent.com/32848391/63605343-09365000-c5ce-11e9-97b8-a5642e71ca24.png)

- press **Import Score** (valid formats: *musescore, MusicXML, MIDI, [PIG](http://beam.kisarazu.ac.jp/~saito/research/PianoFingeringDataset/)*)
- press **GENERATE** (`output.xml` is written)
- press **Musescore** to visualize the annotated score
- press **3D Player** to show the animation (Press `F1` to quit the application)


#### Example output, as displayed in *musescore*:

(If fingering numbers are not clearly visible use `-b` option.)


![bachinv4](https://user-images.githubusercontent.com/32848391/63605352-10f5f480-c5ce-11e9-8b00-34f1adc2e79b.png)

If *vtkplotter* is installed, click on `3D Player` for a visualization. You will see the both hands playing but hear the right hand notes only. 
Chords are rendered as a rapid sequence of notes.

![pianoplayer3d](https://user-images.githubusercontent.com/32848391/63605322-0176ab80-c5ce-11e9-8213-b572d0303523.gif)


## How the algorithm works:
The algorithm minimizes the fingers speed needed to play a sequence of notes or chords by searching 
through feasible combinations of fingerings. 

One possible advantage of this algorithm over similar ones is that it is completely *dynamic*, 
which means that it 
takes into account the physical position and speed of fingers while moving on the keyboard 
and the duration of each played note. 
It is *not* based on a static look-up table of likely or unlikely combinations of fingerings.

Fingering a piano score can vary a lot from individual to individual, therefore there is not such 
a thing as a "best" choice for fingering. 
This algorithm is meant to suggest a fingering combination which is "optimal" in the sense that it
minimizes the effort of the hand avoiding unnecessary movements. 

## Parameters you can change:
- Your hand size (from 'XXS' to 'XXL') which sets the relaxed distance between thumb and pinkie.
- The beam number associated to the right hand is by default nr.0 (nr.1 for left hand). 
You can change it with `-rbeam` and `-lbeam` command line options.
- Depth of combinatorial search, from 2 up to 9 notes ahead of the currently playing note. By
default the algorithm selects this number automatically based on the duration of the notes to be played.

## Limitations
- Some specific fingering combinations, considered too unlikely in the first place, are excluded from the search (e.g. the 3rd finger crossing the 4th). 
- Hands are always assumed independent from each other.
- In the 3D representation with sounds enabled, notes are played one after the other (no chords), 
so the tempo within the measure is not always respected.
- Small notes/ornaments are ignored.


