# glitch-tool

glitch-tool is a simple Python script for messing with files in a few different ways. This tool was created for making glitch art, more specifically doing databending. You can read more about my results in [this blog post](https://tobloef.com/blog/glitch-art/). This tool was mostly created for this one-time use and therefore the code quality isn't great. 

## Usage
```
python glitch_tool.py -i <input-file> -m <mode> -o <output-folder> [options]
```

### `--help` output
```
usage: glitch_tool.py [-h] [-i INFILE] [--infile2 INFILE2]
                      [-m {change,reverse,repeat,remove,zero,insert,replace,move,merge,blend}]
                      [-o OUTDIR] [-s SEED] [-a AMOUNT] [-c CHANGES] [-b BYTES]
                      [-r REPEAT_WIDTH] [-q] [--output-iterations OUTPUT_ITERATIONS]

Do terrible things to data (great for image databending).

options:
  -h, --help            show this help message and exit
  -i INFILE, --infile INFILE
                        Primary input file
  --infile2 INFILE2     Second input file (required for merge/blend modes)
  -m {change,reverse,repeat,remove,zero,insert,replace,move,merge,blend}, --mode {change,reverse,repeat,remove,zero,insert,replace,move,merge,blend}
                        Glitch mode
  -o OUTDIR, --outdir OUTDIR
                        Output folder
  -s SEED, --seed SEED  Seed to use for random
  -a AMOUNT, --amount AMOUNT
                        Amount of new files to create
  -c CHANGES, --changes CHANGES
                        Amount of random changes. Can be in a range, like '1-10'.
  -b BYTES, --bytes BYTES
                        Amount of bytes to change each change. Can be in a range, like '1-10'.
  -r REPEAT_WIDTH, --repeat-width REPEAT_WIDTH
                        Amount of bytes to repeat. Can be in a range, like '1-10'.
  -q, --quiet           Suppress logging
  --output-iterations OUTPUT_ITERATIONS
                        How many iterations between outputs
```

### Modes
The valid modes are:

* `change` - Change bytes in chunk to random values.
* `reverse` - Reverse order of bytes in chunk.
* `repeat` - Repeat first X bytes (specfied with `--repeat-width`) of chunk throughout the chunk.
* `remove` - Remove the chunk entirely.
* `zero` - Make the chunk all zeroes.
* `insert` - Insert random chunk of data at a random point.
* `replace` - Replace chunk with a chunk of random data.
* `move` - Remove a chunk from one position to another.
* `merge` - Copy a random chunk from a second input file (`--infile2`) into the input file.
* `blend` - Average a random chunk from input 1 and input 2 (`--infile2`) for softer hybrid glitching.

## How to play with it
Try these commands and iterate:

```bash
# 1) Gentle corruption
python glitch_tool.py -i photo.jpg -m change -c 3 -b 10 -o ./out/

# 2) Heavy destruction
python glitch_tool.py -i photo.jpg -m move -c 20 -b 200 -o ./out/

# 3) Hybrid two-image merge
python glitch_tool.py -i face.jpg --infile2 city.jpg -m merge -c 12 -b 80 -o ./out/

# 4) Softer two-image blend
python glitch_tool.py -i face.jpg --infile2 city.jpg -m blend -c 12 -b 80 -o ./out/

# 5) Generate multiple variants with fixed randomness
python glitch_tool.py -i photo.jpg -m reverse -c 5-15 -b 20-120 -a 8 -s 1234 -o ./out/
```

Tips:
- Start with JPG/PNG copies, never originals.
- Increase `-c` (changes) and `-b` (bytes) for stronger glitches.
- Use ranges (for example `-c 5-20 -b 10-200`) to get varied results.
- Use `-s` to reproduce a result exactly.
