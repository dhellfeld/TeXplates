# UC Berkeley Dissertation template using LaTeX and make
D. Hellfeld (01.20.18)

Style/class file from: https://math.berkeley.edu/~vojta/tex/ucbthesis-phd.html

### Organization/File structure
The main TeX document lives in the root directory while all other sections are organized into directories and subdirectories. 

```
./
│ README.md
│ thesis.tex    
│ Makefile
|── FrontMatter/
│  |── title/
│  |  │ title.tex
│  |── approval/
│  |  │ approval.tex
│  |── copyright/
│  |  │ copyright.tex
│  |── abstract/
│  |  │ abstract.tex
│  |── dedication/
│  |  │ dedication.tex
│  |── indexing/
│  |  │ indexing.tex
│  |── acknowledgements/
│     │ acknowledgements.tex
|── Chapters/
│  |── ch1/
│  |  │ ch1.tex
│  |── ch2/
│  |  │ ch2.tex
│  |── ...
|── Appendices/
│  |── appendixA/
│  |  │ appendixA.tex
│  |── appendixB/
│  |  │ appendixB.tex
│  |── ...
|── References/
|  | references.tex
|── Outline/
|  | outline.tex
|── Style/
|  | ucbthesis.cls
|  | variables.sty
```

- The `indexing.tex` file contains the commands for making the table of contents and list of figures. These commands can really be placed into another file, but I find it useful to compartmentalize everything.
- The `variables.sty` file contains user-defined LaTeX variables that can be used throughout the document. By default this is where information on the author, institution, committee, degree etc. is defined.
- The `outline.tex` file is there if you find outlines useful, but it is not necessary for building the thesis. 
- As additional chapters and appendices are created, the appropiate directories will need to be created and names added to the makefile. Based on the existing template, it should be clear on how to do this.

### Compilation
The makefile is designed to facilitate the building of the whole document and/or individual sections. This is done via commands such as 

`$ make thesis`
`$ make ch1`
`$ make appendixB`
`$ make abstract`

and so on. The command `$ make help` can be run to see all the available commands. 

All of the LaTeX build files (including PDFs) will be placed into a directory named `_build/` in the root directory by default. This directory will follow the same file structure as above. When individual sections are built, they will contain their own build directires with their own build files. The command

`$ make clean`

will simply remove the build directory.


### References
The standard UC Berkeley thesis places the references from the entire document at the end. When compiling standalone sections for review, the makefile will compile references for each individual section.
