## Install Dorado

Be sure to get the correct version for your system. The following instructions are for macOS ARM64 (Apple Silicon). If you are using a different system, please refer to the [Dorado installation guide](https://nanoporetech.github.io/dorado/installation.html).

```
cd ~/bin
wget https://cdn.oxfordnanoportal.com/software/analysis/dorado-1.0.2-osx-arm64.zip
unzip dorado-1.0.2-osx-arm64.zip

# add dorado to PATH in fish (this will be different if you use bash or zsh, i.e. most users)
# if you use bash or zsh, you can add the following line to your ~/.bashrc or ~/.zshrc file:
# export PATH=$PATH:~/bin/dorado-1.0.2-osx-arm64/bin

# for fish shell users, run the following command to add dorado to PATH
set -Ux fish_user_paths $fish_user_paths ~/bin/dorado-1.0.2-osx-arm64/bin
```

## test the installation

```
cd ~/git/Aditi-polyA
dorado aligner --help
```

## install [pod5 tools](https://pod5-file-format.readthedocs.io/en/0.1.21/docs/install.html)
 This allows me to subset the pod5 file to a smaller size for testing.
```
pip install pod5
```

## subset the pod5 file
I use head and tail to get reads in the middle of the file, just in case.
```
cd ../input
pod5 view PBC42138_pass_barcode01_c0ea801a_472d12cd_0.pod5 --ids --no-header > readIDs.txt

# make a csv file with the output file name and read IDs
head -n 100000 readIDs.txt | tail -n 10000 | sed -E "s/^/sample.pod5,/" > readIDs_small.csv

pod5 subset --force-overwrite  --csv readIDs_small.csv ./PBC42138_pass_barcode01_c0ea801a_472d12cd_0.pod5
```



## Run basecaller

```
## paths assume you are in `scripts` directory
dorado basecaller --verbose --reference /Users/jmaloof/Sequences/ref_genomes/A_thaliana/TAIR10_chr_ALL.fasta --estimate-poly-a  --poly-a-config ../input/polya_config.cdna.toml --output-dir ../output fast ../input/sample.pod5
```

