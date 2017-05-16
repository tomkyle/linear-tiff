# negatives/linear-tiff

**Converts RAW/NEF/CR2 files into linear 16bit-TIFF files.**

**Features:**

- **Mirroring** for negatives digitalized on their emulsion side. 
- **Resizing**  when megapixel power is not everything.
- **Grayscaling:** B/W lovers save up to 60% disk space. 
- **ICC profiles:** Output images get a linear gamma profile.  
  No ‘custom gamma colorspace’ in Photoshop!

**What happens inside?** [dcraw](cybercom.net/~dcoffin/dcraw/dcraw.1.html) is used to create linear 16bit TIFF files. [ImageMagick](https://www.imagemagick.org/script/index.php) then does the color-profiling, B/W grayscaling, mirroring, resizing, and ZIP compression. [GNU Parallel](https://www.gnu.org/software/parallel/) uses all CPU cores to speed up the whole thing.


## Homebrew Installation (Mac OS)


The *linear-tiff* bash script can be installed by a [Homebrew](https://brew.sh/) formula, which itself is part of the [tomkyle/homebrew-negatives](https://github.com/tomkyle/homebrew-negatives) tap. 

```bash
# Optional: Install tap
$ brew tap tomkyle/negatives

# Install formula
$ brew install linear-tiff
```

As “tapping” first is not neccessarily needed, you can install the formula directly:

```bash
$ brew install tomkyle/negatives/linear-tiff
```

# Usage

Run `linear-tiff --help` or `-h` to display help text. See [options](#options) and [examples](#examples).

```bash
$ linear-tiff [options] [-a | file(s)]
```



## Options

Option | Value | Description
:------|:------|:------------
`-a, --all`     |       | All images — process any RAW/NEF/CR2 file in working directory, using GNU Parallel.
`-d, --desaturate`     |       | Desaturate colors — recommended for B/W negatives. dcraw's TIFF output is converted to 16-bit grayscale, with a linear gamma 1.0 ICC profile applied (Gray-elle-V4-g10.icc). Grayscaling saves up to 60% in file size. 
`-f, --flipflop`     | value | Mirror the image vertically and/or horizontally. Possible values are `flop` horizontal, `flip` vertical or even `flipflop` (guess what). Example: `-f flop`
`-h, --help`     |       | Display help text
`-o, --output`     | path  | Output directory — default is current working directory. Example: `-o results`
`-r, --resize`     | pixel | Resize  image — pixel width for larger side, preserving aspect ratio. Example: `-r 3000`
`-v, --verbous`     |       | Verbous mode — show some more information under way.


## Examples

**Convert all images** or **list of images** in current working directory using defaults:

```bash
# All images:
$ linear-tiff -a 
$ linear-tiff --all

# List of images:
$ linear-tiff DSC0001.NEF DSC0002.NEF DSC0003.NEF
```

**Desaturate** B/W negatives:

```bash
# These are equal:
$ linear-tiff -d DSC0001.CR2 DSC0002.CR2
$ linear-tiff --desaturate DSC0001.CR2 DSC0002.CR2

# And those as well:
$ linear-tiff -ad
$ linear-tiff -a --desaturate
$ linear-tiff --all -d
$ linear-tiff --all --desaturate
```

**Resize images:**

```bash
# These are equal:
$ linear-tiff -r 2048 DSC0001.CR2 DSC0002.CR2
$ linear-tiff -a --resize 2048
```

**Fullstack Conversion:**  
Resized and horizontally mirrored B/W versions of all images go into ‘fullstack’ directory. Verbous output is showing what's going on.

```bash
# These are equal:
$ linear-tiff -adv -r 2048 -f flop -o fullstack
$ linear-tiff --all --resize 2048 --flipflop flop --desaturate --output fullstack --verbous 
```


# Changelog

## New Features

- **Long option names** for those preferring self-explanatory options like `--resize`, `--desaturate` and so on.



## Upcoming Features

These features go into the current major version 1:

- **Star rating filter:** Many photo managers like Lightroom or Bridge let their users reject bad images or rate better ones with ‘stars’. *linear-tiff* will get a new CLI option flag to set a minimum rating level. 

- **Custom configuration files:** Would it not be fine if users could store their favourite options in a configuration file? `~/.negativesrc` or ` ~/linear-tiff.conf` or even an *INI, YAML* or *JSON?*



## Roadmap to version 2


- **The *-r* option will be renamed to *-w*,** as *-r* is more natural for the upcoming *Rating filter*, and so is *-w* for *width*.

- **The *-f* option will be renamed to *-m*,** as the actually performed image action is *mirroring horizontally or vertically*. The option values *flip, flop* and *flipflop* will then become something like *V*, *H* or *VH*.

- **New batch mode trigger:** New sub-command `all` will replace the current `-a` flag, like so: `linear-tiff batch <options>`. 


                              
# Issues and FAQ

To see the full list, head over to the [issues page.](https://github.com/tomkyle/negatives-linear-tiff/issues)

**linear-tiff does not find my Raw photos in batch mode.**  
Currently, *linear-tiff* uses a regex to locate RAW files by these extensions: NEF (Nikon), CR2 (Canon), and RAW (Contax, Kodak, Leica, Panasonic). Leave a comment on [issue#2](https://github.com/tomkyle/negatives-linear-tiff/issues/2) to “order” your favourite file extension. I'll happily lengthen the regex to your needs :smiley:


**I get a error message “mogrify: delegate library support not built-in”**  
ImageMagick must be compiled with litte-cms2 support. See [issue#1](https://github.com/tomkyle/negatives-linear-tiff/issues/1) for details. 



# Development and Contribution

```bash
$ git clone https://github.com/tomkyle/negatives-linear-tiff.git
```

