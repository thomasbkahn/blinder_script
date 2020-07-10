# blinder_script
A simple script for blinded analysis. Assumes structure where data to be analyzed has already been
split into separate files, each of which is subject to varying experimental treatment. The script
copies the files to a new location, replacing the original filenames with a randomly-assigned
number, saving a plaintext key that can be used to automatically rename the files with their
original value. An analyst processing this data will not have any indication of each files status
and thus can provided blinded, unbiased analysis of the results.

## Disclaimer

**BACK UP YOUR DATA!** This script is intended to be non-destructive, but I have not given it much
attention since writing it years ago - back up your data just in case.

## Usage

*Tested on python 3.8, probably works on any python 3.x version though.*

Run the script as `/path/to/blinder /path/to/target_directory`.

The contents of `/path/to/target_directory` will be copied to a new directory
`/path/to/target_directory_blinded` (meaning, a new directory is made in the same parent directory
of your target directory, and the suffix "_blinded" is added). Each of the input files will be
copied to this new directory, but renamed with a random choice between 0 and n - 1 (where n is the
number of files in the target directory). That means this script is *non-destructive* (though I
recommend backing up your data prior to running this script anyways). File extensions are
maintained, so if that can reveal something you'd like blinded you'll need to find some workaround.
When you're done with your analysis, repeat the same call on the **original target directory**, and
you'll be asked whether you want to unblind. Typing "yes" will rename the files in the "_blinded"
directory to their original name but with the suffix "_processed" added before the extension. The
intention here is that if your analysis requires changes to the files, you can merge these back in
with the originals. **If you delete the key file, you will not be able to unblind the results (and
unless you recorded this information elsewhere, you may not be able to map the blinded filenames
back to their originals).**

Illustrating the above with an example:

Let's say your original directory looks like this:

```
./test_data/
├── control_001.png
├── control_002.png
├── control_003.png
├── control_004.png
├── experiment_001.png
├── experiment_002.png
├── experiment_003.png
└── experiment_004.png
```

After running the script, there is now a new directory called `test\_data\_blinded`, which looks
like:

```
./test_data_blinded/
├── 0.png
├── 1.png
├── 2.png
├── 3.png
├── 4.png
├── 5.png
├── 6.png
├── 7.png
└── key_test_data.txt
```

Next, after you complete your analysis and run the blind script again (on the **original
directory**), the blinded directory will look like:

```
./test_data_blinded/
├── control_001_processed.png
├── control_002_processed.png
├── control_003_processed.png
├── control_004_processed.png
├── experiment_001_processed.png
├── experiment_002_processed.png
├── experiment_003_processed.png
├── experiment_004_processed.png
└── key_test_data.txt
```

If you don't need the file copies (as in, you haven't made any edits but just recorded analysis
elsewhere) you can just use the keyfile directly to map the blinded filenames to their original.
The file in this example is:

```
1.png   experiment_002.png
6.png   experiment_001.png
0.png   control_003.png
2.png   experiment_003.png
4.png   control_002.png
7.png   control_001.png
5.png   experiment_004.png
3.png   control_004.png
```

The left column provides the blinded filename and the right column provides the original name.

## Todos

This script was made years ago, before I had learned the value of CLIs and logging.
*Realistically, I will never work on any of this - but PR's are welcome!*

- CLI interface, using `argparse`. The unblind command can then be more explicit. 
- key data in conventional format, e.g. csv or json if plaintext is still required, or pickle
  otherwise
- hide keyfile by (optionally) add `.` prefix to filename, and / or optionally save as binary /
  encoded file
- provide option for destination of blinded directory (can keep current behavior as default).
