# flactrack

Flactrack is a shell script for splitting single-file albums into tagged flac tracks using a cue file. It uses only the command line flac encoder/decoder and standard utilities, and is meant to be portable and robust. It supports the same set of input formats as the flac encoder.

## Usage

    flactrack [options] <file.cue>

#### File Selection
The sript expects one cue file as a command line argument. The name of the matching audio file is read from the cue file. If that audio file cannot be found (perhaps because it has been renamed and the cue file has not been updated accordingly), the script will look for an audio file that's likely to be the right one based on its name. If the script can't find the matching audio file, simply edit the FILE line in the cue file with your favorite text editor so that it contains the correct file name.

##### Supported and Unsupported Audio Formats
Flactrack only supports those audio formats that are supported by the flac encoder. If the audio file to be split is not a .flac file, the script will attempt to use the flac encoder to create a temporary flac file and split that one instead. This lengthens the job considerably. See the flac encoder's documentation for a list of supported input formats. Because .wav is one of the supported formats, splitting an unsupported file type (e.g. .ape) is as simple as running the appropriate decoder first:

    mac <file.ape> <file.wav> -d && flactrack <file.cue>
    # or, alternatively:
    ffmpeg -i <file.ape> <file.flac> && flactrack <file.cue>

Flactrack should be able to find the decoded file automatiaclly. Similarly, converting the resulting tracks is relatively simple. In this case using ffmpeg has the advantage of preserving tags:

    mkdir MP3; for t in [0-9][0-9]\ -\ *.flac; do ffmpeg -i "$t" -ab 320k MP3/"${t%.flac}.mp3"; done

#### Tagging
Created tracks are tagged with metadata from the cue file and a cover image. If the metadata in the cue is incorrect or missing, the equivalent tags will be as well. You can change the content of the tags by editing the cue file before you run the script.

In order for the script to find it automatically, the cover image must be a jpg or a png file called "cover", "folder", "front," (case insensitive) or the hidden equivalent (prepended with a dot) and have an appropriate file extension. The script looks for jpg first, then png, and uses the first match it finds. Alternatively, you can specify an image to use on the command line with  
"--image=\<file\>". If the specified file does not exist the option will be ignored and the script will look for a suitable image in the usual manner. Image tagging can be disabled by selecting an empty string (--image="").

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

##### Dependencies and Compatibility
Besides the Xiph.org command line flac encoder/decoder program, flactrack relies almost exclusively on POSIX-defined shell syntax, files, variables, and utilities. Currently, the exceptions to this rule are the "-o" and "-A" options for grep and the  
"-maxdepth" and "-iname" options for find. If your grep and find have these options and your system is reasonably POSIX compliant, you should be able to run flactrack. 
