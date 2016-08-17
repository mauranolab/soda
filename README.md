# soda.py
Python-based soda-plot gallery generator

- [Description](#description)
- [Dependencies](#dependencies)
- [Usage](#usage)
- [Options](#options)

## Description

`soda.py` is a Python script that generates a gallery of images made from snapshots from a UCSC genome browser instance, so-called "soda plots". Snapshots could be derived from the Altius internal browser instance `gb1`, or any other UCSC browser instance, if specified.

You provide the script with a few parameters:

1. A BED-formatted file containing your regions of interest.
2. The [session ID](https://genome.ucsc.edu/goldenpath/help/hgSessionHelp.html) from your genome browser session, which specifies the browser tracks you want to visualize, as well as other visual display parameters that are specific to your session.
3. Where you want to store the gallery end-product.

If the BED file contains a fourth column (commonly used to store the [name](https://genome.ucsc.edu/FAQ/FAQformat.html#format1) of the region), its values are used as labels for each page in the gallery.

Additional options are available; please see the [Options](#options) section.

### Note

The BED file does not need to be in BEDOPS `sort-bed` order. In fact, it can be useful to order the regions in a BED file by some criteria other than genomic position, such as some numerical value stored in the BED file's score column, *e.g.*:

```bash
$ sort -k5,5n input.bed > input_sorted_by_scores.bed
```

Any ordering is allowed.

## Download

To grab this kit, you can clone it from Github:

```bash
$ git clone https://github.com/Altius/soda.git
```

## Dependencies

From an Altius HPCA cluster schedule host (`sched0.altiusinstitute.org`), add two modules to satisfy some dependencies, before running `soda.py`:

```bash
$ module add anaconda
$ module add ImageMagick
```

Both of these modules are required to run `soda.py` from this host.

### Note

You do not necessarily need to run this from `sched0`. However, if you run this script locally, such from another host in the lab, or on your personal workstation or laptop, you may need to use `pip` or similar to add Python libraries like [Beautiful Soup](https://pypi.python.org/pypi/beautifulsoup4) and [Jinja2](https://pypi.python.org/pypi/Jinja2), as well as a package manager to install [ImageMagick](http://www.imagemagick.org). Installing these dependencies may also require administrator privileges. In this case, please see the documentation for these components for installation instructions, or contact IT support for assistance.

## Usage

As a usage example, you may have a BED file in some home directory called `/home/abc/regions.bed`. You have a session ID for the `gb1` Altius genome browser instance called `123456_abcdef`, with all your tracks selected and display parameters set. Finally, you want to store the results in a folder called `/home/abc/public_html/my-soda-plot-results` where it can be shared publicly with others via a web browser:

```bash
$ /path/to/soda.py -r "/home/abc/regions.bed" -s "123456_abcdef" -o "/home/abc/public_html/my-soda-plot-results"
```

When finished, the final product is a gallery that is available from the web address `http://www.uwencode.org/~abc/my-soda-plot-results`.

For example, here is a gallery made from earlier testing that is stored in my home directory's `public_html` folder: [http://uwencode.org/~areynolds/soda-test/](http://uwencode.org/~areynolds/soda-test/)

If you run this locally, you can open the result folder's `index.html` file with your web browser to load the gallery. For example, from the Terminal application in OS X, you can run:

```bash
$ open /Users/abc/my-soda-plot-results/index.html
```

which opens the gallery index in the default web browser.

## Options

Other options are available depending on how you want to customize the run.

```bash
-t, --title
```

Use `-t` or `--title` to specify a gallery title.

```bash
-a, --range
```

Use the `-a` or `--range` option to pad the BED input symmetrically by the specified number of bases. This is similar to applying operations with BEDOPS `bedops --range`, except that this works regardless of the sort order of the input.

```bash
-b, --browserBuildID
```

Use the `-b` or `--browserBuildID` option to specify the genome build key other than the default, *e.g.* `hg19` or `mm10`. The key `hg38` is used as the default.

```bash
-g, --browserURL
```

Use the `-g` or `--browserURL` option to specify a different genome browser URL other than the Altius `gb1` host.

```bash
-u, --browserUsername
-p, --browserPassword
```

Use these two options if you pick a different `--browserURL` to specify authentication credentials.

```bash
-v, --verbose
```

Use `-v` or `--verbose` to print debug messages, which may be useful for automation or debugging.