---
title: "NautilusSVN: Linux TortoiseSVN Equivalent"
date: "2009-01-04"
tags: [linux,open-source,python,tools]
---

Based on [Stuart Langridge's](http://www.kryogenix.org/days/2006/09/12/extremely-noddy-tortoisesvn-for-the-gnome-desktop) original script, [Jason Field](http://www.jasonfield.com/) and Bruce van der Kooij created a set of Python scripts which integrate a load of Subversion functionality into the Gnome Nautilus browser. It's basically a clone of the TortoiseSVN project on Windows.

NautilusSVN currently supports the following functionality:

- Checkout
- Commit
- Revert
- Diff (using Meld or gvimdiff)
- Log Viewer
- Revision and SVN User as columns in Nautilus views
- Emblems to show file status (though buggy)
- SSL authentication (buggy)
- Username and password entry dialog
- Editing Properties

[NautilusSVN Project](http://code.google.com/p/nautilussvn/)
