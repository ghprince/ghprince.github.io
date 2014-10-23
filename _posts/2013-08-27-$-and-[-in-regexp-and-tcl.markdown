---
layout: post
title : "$ and [ in Regexp and Tcl"
date  : 2013-08-27 19:26:00
---

I use mainly Tcl/Expect in my daytime job, it is due to the legacy system we use and also Tcl/Expect is just good enough to fit so that nobody want to change or upgrade.

Recently I have been fighting with regular expression a lot, and to be specfic, regexp in Tcl. And because my work involves a lot string parsing of command lines and console outputs, there are many special charactors need extra attention.

**$** and **[** are two of those charactors.

For **$**, to match a literal **$**, we need to first turn off substitution in Tcl, then turn off end-of-line match in regexp, both by **\\**, but **\\** itself needs to be escaped by another **\\** as well, so we need three **\\** s. Therefore,

    expect -re "\\\$foo"    # matches literal $foo
    expect -re "\\$foo"     # won't match anything, because $ means end-of-line here
    expect -re "\$foo"      # won't match anything, because $ means end-of-line here
    expect -re "$foo"       # matches the value of variable foo

we can turn off substitution in Tcl by {}, so

    expect -re {\$foo}      # matches literal $foo
    expect -re {$foo}       # won't match anything, because $ means end-of-line here

However if the character after "$" is non-alphabet, the subsitituion is turned off automatically, so

    expect -re "\\\$"       # matches literal $
    expect -re "\\$"        # matches literal $
    expect -re "\$"         # matches anything, because $ means end-of-line here

Similar rule applies to **[** , because it also has special meaning in both Tcl and regexp. 