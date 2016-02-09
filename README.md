# flactrack

Flactrack is a shell script for splitting single-file albums into tagged flac tracks using a cue file. It uses only the command line flac encoder/decoder and standard utilities, and is meant to be portable and robust.

## Usage

    flactrack [options] <file.cue>

#### File Selection
The sript expects one cue file as a command line argument. The name of the matching audio file is read from the cue file. If that audio file cannot be found (perhaps because it has been renamed and the cue file has not been updated accordingly), the script will look for an audio file that's likely to be the right one based on its name. If the script is unable to find the matching audio file, simply edit the FILE line in the cue file with a text editor so that it contains the correct file name and run the script again.

##### Supported and Unsupported Audio Formats
Flactrack only supports those audio formats that are supported by the flac encoder. If the audio file to be split is not a .flac file, the script will attempt to use the flac encoder to create a temporary flac file and split that one instead. This lengthens the job considerably. See the flac encoder's documentation for a list of supported input formats. Because .wav is one of the supported formats, splitting an unsupported file type (e.g. ape) is as simple as running the appropriate decoder first:

    mac <file.ape> <file.wav> -d && flactrack <file.cue>
    # or, alternatively:
    ffmpeg -i <file.ape> <file.flac> && flactrack <file.cue>

Flactrack should be able to find the decoded file automatiaclly. Similarly, converting the resulting tracks is relatively simple. In this case using ffmpeg has the advantage of preserving tags:

    mkdir MP3; for t in [0-9][0-9]\ -\ *.flac; do ffmpeg -i "$t" -ab 320k MP3/"${t%.flac}.mp3"; done

##### Embeded Cue Sheets
Flac supports embedding cue sheets in audio files. The "metaflac" command can be used to extract an embedded cue sheet to a file:

    metaflac --export-cuesheet-to=<file.cue> <file.flac>
Note that embedded cue sheets only retain indexing information, and so using an extracted cue sheet to split a file will result in tracks that are not tagged, except maybe for a cover image.

#### Tagging
Created tracks are tagged with metadata from the cue file and a cover image. If the metadata in the cue is incorrect or missing, the equivalent tags will be as well. You can change the content of the tags by editing the cue file before you run the script.

The cue specification contains the tags TITLE, PERFORMER, and SONGWRITER which are mapped to the flac tags TITLE, ARTIST, and COMPSER respectively, excpet for the per-album TITLE, which is mapped to the ALBUM tag. If there is both a per-album and a per-track PERFORMER tag in the cue sheet, the per-track PERFORMER is used for the ARTIST tag and the per-album PERFORMER is used as the ALBUMARTIST tag only if it is different from the per-track PERFORMER. If there is no per-track PERFORMER, the per-album PERFORMER is used for the ARTIST tag. By convention, cue sheets also often contain tags for DATE, GENRE, DISCNUMBER, and TOTALDISCS using the REM \<comment\> syntax (e.g. "REM DATE 2001"). DATE and GENRE are always translated to tags, but DISCNUMBER will only be used if TOTALDISCS is more than one, and TOTALDISCS is not used except in deciding whether or not to use DISCNUMBER.

In order for the script to find it automatically, the cover image must be a .jpg or a .png file called "cover", "front_cover" "folder", "front", (case insensitive) or the hidden equivelent of one of those (prepended with a dot). The script looks in the cue file's directory and any immediate subdirectories for jpg first, then png, and uses the first match it finds. Alternatively, you can specify an image on the command line with "--image=\<file\>". If the specified image does not exist, the option will be ignored and the script will look for a suitable image in the usual manner. Image tagging can be disabled by selecting an empty string (--image="").

#### Options
     -h, --help     display this help and exit  
     -g, --pregap   prepend pregaps, rather than appending  
                     them to the previous track  
     -f, --force    overwrite existing files, rather than  
                     skipping them  
     -0 ... -8      set the compression level; default is 5  
     --image=<file> select an image to tag the tracks with  
     -i <file>      short for --image  
Options must be declared seperately. Strings of short options are not supported.

##### File Removal
Flactrack does not have an option to remove the original file after splitting it into tracks. This behavior is very easy to produce using '&& rm':

    flactrack <file.cue> && rm <file.flac>
Because the script only exits with status 0 if all of the tracks are created successfully, this will remove \<file.flac\> only under that condition.

### Installation, Updates, and Uninstallation
Installing the script is as simple as downloading it, making it executable, and putting it in your PATH. Make sure that you also have the Xiph.org command line flac encoder/decoder program. For updates, repeat the installation process, overwriting the original file. To uninstall the script, simply delete the file.

    # Download:  
    curl https://raw.githubusercontent.com/mf-luder/flactrack/master/flactrack -o ~/flactrack
    # or, alternatively:
    wget https://raw.githubusercontent.com/mf-luder/flactrack/master/flactrack -O ~/flactrack
    
    # Install:
    chmod +x ~/flactrack && sudo mv ~/flactrack /usr/local/bin/flactrack
    
    # Remove:
    sudo rm /usr/local/bin/flactrack
