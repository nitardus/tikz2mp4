tikz2mp4
========

This is a simple script which converts tikz animations to mp4 videos,
mainly intended to make them truly portable (e.g. in order to reuse
the in Powerpoint presentations. It works by altering the input
tex-file(s) to use the snapshot functionality of tikz to render each
frame as a single picture and then piping the result to the latex
engine. For efficiency reasons, it uses concurrent child processes to
do this work in parallel. After that, the results are written to a
temp-directory, converted to png using imagemagick and then finally
cast into a video using ffmpeg, which get copied back to the original
directory.

Installation
------------
Make sure that you have the following requirements installed

	imagemagick
	ffmpeg
	perl
	
In addition to this, you need the IPC::Run perl module. It is
advisable to use the version packaged by your system, for example

	pacman -S perl-ipc-run # Arch
	apt install libipc-run-perl # Debian
	
Or, failing this, you can always use CPAN

	sudo cpan IPC::Run
	
After that, you can either execute the script directly with its full
path or copy it somewhere in your $PATH and execute it with

	tikz2mp4

Usage
-----

The script requires you at least to specify the duration (in seconds)
of the video, as it cannot guess this from the tikz source:

	tikz2mp4 -l 5.5 example.tex

You can further specify the resolution and framerate; see the usage note

	tikz2mp4 -h
	
Please note that it is advisable to use a framerate of 50 or 25 over a
framerate with 60 or 30 fps, because the resulting fractions have to
be truncated to be used with TeX.

The input TeX file has to have the following outline

	\documentclass{standalone}
	\usepackage{tikz}
	\usetikzlibrary{animations}

	\begin{document}
	\thispagestyle{empty} % optional, but advisable
	\begin{tikzpicture}   % or       \begin{tikzpicture}[
	                      % but not: \begin{tikzpicture}[<option>]
	
	... your .tikz animation
	
	\end{tikzpicture}
	\end{document}

See the included example.tex

Limitations
-----------

This script developed and tested on Linux, but should in theory work
on MacOS and Windows too. Feel free to try it out and to report and
issues!

