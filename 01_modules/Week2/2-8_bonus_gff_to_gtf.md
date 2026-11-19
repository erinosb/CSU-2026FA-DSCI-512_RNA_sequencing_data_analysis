# .gff to .gtf conversion

Some annotation files are only available as .gff files. 

If this is the case, we suggest using:

[https://github.com/gpertea/gffread](https://github.com/gpertea/gffread)

Download the software for gffread using their instructions.

Here is how it can be run:

```bash
gffread <original gff3 filename> -T -o <output gtf file name>
gffread Arabidopsis_thaliana.TAIR10.49.gff3 -T -o gffread_Arabidopsis.gtf
(-T -o option will output GTF format instead of GFF3)
```

Here's some more info on it: [https://manpages.ubuntu.com/manpages/resolute/en/man1/gffread.1.html](https://manpages.ubuntu.com/manpages/resolute/en/man1/gffread.1.html)

