Andor Journals
==============
Documentation and source code of journals for streamlining control of Andor
hardware: FRAPPA, Mosaic, Micropoint, Laser Combiner

Journals, being XML based, are not human-readable outside of Metamorph.  To
work around this the `doc_jnl.py` generates Python-like source code from the
XML for each directory's README

Installation
------------
1.  [Download](journals/zipball/master) the zip file

2.  Copy folders as needed to Metamorph's journals directory, usually:
```
C:\MM\app\mmproc\journals\
```

3.  Browse instructions in each folders' README.  This is best viewed online
    where the Markdown format is rendered
