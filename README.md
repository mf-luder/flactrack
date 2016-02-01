# flactrack

Flactrack is a shell script for splitting single-file albums into tagged flac tracks using a cue file. It uses only the command line flac encoder/decoder and standard utilities, and is meant to be portable and robust. It supports the same set of input formats as the flac encoder.

## Usage

    flactrack [options] <file.cue>

#### File Selection
The sript expects one cue file as a command line argument. The name of the matching audio file is read from the cue. If that file cannot be found (perhaps because the audio file has been renamed and the cue file has not been updated accordingly), the script will attempt to guess at which audio file is the right one by seeing if there's only one audio file in the directory with the right file extension or if there's an audio file with the same name as the cue file. If the script can't find the matching audio file, simply edit the FILE line in the cue so that it contains correct file name.

#### Tagging
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
Strings of short options are not supported. Options must be declared seperately.

### Installation, Updates, and Uninstallation
Installing the script is as simple as downloading it, making it executable, and putting it in your path (for Linux I recommend /usr/local/bin/). Make sure that you also have the Xiph.org command line flac encoder/decoder program. For updates, repeat the installation process, overwriting the original file. To uninstall the script, simply delete the file.

    # Download:  
    curl https://raw.githubusercontent.com/mf-luder/flactrack/master/flactrack -o ~/flactrack
    # or
    wget https://raw.githubusercontent.com/mf-luder/flactrack/master/flactrack -O ~/flactrack
    
    # Install:
    chmod +x ~/flactrack && sudo mv ~/flactrack /usr/local/bin/flactrack
    
    # Remove:
    sudo rm /usr/local/bin/flactrack
