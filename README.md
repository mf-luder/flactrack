# flactrack
A bash script for splitting a lossless single-file album into tagged flac tracks using the cue.
It uses only the command line FLAC encoder/decoder and standard utilities.

The sript expects one cue file as a command line argument. The name of the matching audio file is read from the cue. If the audio file has been renamed and the cue file has not been updated accordingly, the script will attempt to guess at which audio file is the right one by seeing if there's only one audio file in the directory or if one of the audio files in the directory has a name that is within the directory name. If the script can't find the matching audio file, simply edit the TITLE line of the cue file that appears somewhere above the first TRACK line so that the cue file contains the correct file name.

Created tracks are tagged with metadata from the cue file and a cover image. If the metadata in the cue is incorrect or missing, the equivalent tags will be as well. You can change the content of the tags by editing the cue file before you run the script.

The cover image must be called "cover.*", "folder.*", "front.*", or the hidden equivalent (prepended with a dot). The format can be jpg or png. If there are multiple matches, jpg is preferred over png. Within each format, the most explicit name ("cover")  is the most preferred and the least explicit ("front") is the least preferred. Alternatively, you can specify an image to use on the command line with "--picture=file.jpg".

INSTALLATION: chmod +x flactrack && sudo mv flactrack /usr/local/bin/

UNINSTALLATION: sudo rm /usr/local/bin/flactrack
