# flactrack

Flactrack is a shell script for splitting a lossless single-file album into tagged flac tracks using a cue file. It uses only the command line FLAC encoder/decoder and standard utilities, and is meant to be portable and robust.

## Usage

    flactrack [options] \<file.cue\>

#### File Selection and Tagging
The sript expects one cue file as a command line argument. The name of the matching audio file is read from the cue. If that file cannot be found (perhaps because the audio file has been renamed and the cue file has not been updated accordingly), the script will attempt to guess at which audio file is the right one by seeing if there's only one audio file in the directory or if one of the audio files in the directory has a name that is within the directory name. If the script can't find the matching audio file, simply edit the FILE line in the cue so that it contains correct file name.

Created tracks are tagged with metadata from the cue file and a cover image. If the metadata in the cue is incorrect or missing, the equivalent tags will be as well. You can change the content of the tags by editing the cue file before you run the script.

In order for the script to find it automatically, the cover image must be called "cover.\*", "folder.\*", "front.\*", or the hidden equivalent (prepended with a dot). The file extension must be jpg or png. If there are multiple matches, jpg is preferred over png. Within each format, the most explicit name ("cover")  is the most preferred and the least explicit ("front") is the least preferred. Alternatively, you can specify an image to use on the command line with "--image=file.jpg". If the specified file does not exist the option will be ignored and the script will look for a suitable image in the usual manner. Image tagging can be disabled by selecting an empty string (--image="").

#### Options
     -h, --help     display this help and exit  
     -g, --pregap   prepend pregaps, rather than appending  
                     them to the previous track  
     -f, --force    overwrite existing files, rather than  
                     skipping them  
     -0 ... -8      set the compression level; default is 5  
     --image=<file> select an image to tag the tracks with  
     -i <file>      short for --image  


### Installation, Updates, and Uninstallation
Installing the script is as simple as downloading it, making it executable, and putting it in your path (for Linux I recommend /usr/local/bin/). Make sure that you also have the Xiph.org command line flac encoder/decoder program. For updates, repeat the installation process, overwriting the original file. To uninstall the script, simply delete the file.

    chmod +x flactrack && sudo mv flactrack /usr/local/bin/

    sudo rm /usr/local/bin/flactrack

Right now the sript is still being tested and is changing frequently, so check back often for updates.
