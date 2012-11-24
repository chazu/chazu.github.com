layout: post
title: "Urwid and You: Part One"
category: 
tags: []
---
{% include JB/setup %}

 - What urwid is

   \[Urwid\](http://excess.org/urwid/) is an open-source text-based user interface library written for use in Python by Ian Ward. It's quite feature-rich, and very well documented, however for users of curses it may be a bit difficult to adjust at first. Let's take a look at some of the very basic elements of programming with urwid, starting with installation.
 
# Installing urwid

   Urwid can be downloaded from its \[homepage\](http://excess.org/urwid/) and the source can be obtained from github or through the project's Mercurial repository. Additionally, ubuntu users should be able to install the library using the command:

   sudo apt-get install urwid-python

or

   sudo apt-get install urwid-python3

depending on the version of python being used.

Finally, urwid can be installed via the pip package manager. Just type 'pip search urwid'.

# Hello world!

   Now that we've got urwid installed, let's get this over with. Here's your hello_world.py:

>   import urwid
>
>   text = urwid.Text("Hello World!")
>
>   container = urwid.Filler(text)
>
>   root = urwid.MainLoop(container)
>
>   root.run()

   This is fairly self-explanatory, however let's check it out. After importing the library, we create a text widget, which is roughly analagous to a label for a text box in HTML or tkinter.  Next, we slap that puppy inside of a Filler, which is what urwid calls a 'box widget', and we'll get into all that stuff soon. We then instantiate an event loop, which takes a single widget as an argument. This is known in Urwid-Land as the 'top' widget. We then start the event loop using our container. Easy as pie.

#Widget types

   There are three basic types of widgets in urwid: Box, Flow and Fixed. Let's explore the differences with a few code examples. There will be gotchyas, even for some pretty basic cases, so bear with me.

## Flow Widgets

   A flow widget is one which can be displayed on a variable number of rows or columns. Let's try an experiment:

>   import urwid
>
>   butten = urwid.Button("Release the Kraken!")
>
>   container = urwid.Filler(butten)
>
>   root = urwid.MainLoop(container)   
>
>   root.run()

DON'T PRESS THE BUTTON!  Once you've run this script, add a couple of lines:

>   import urwid
>
>   butten = urwid.Button("Release the Kraken!")
>   other_butten = urwid.Button("Pew Pew Lasers")
>
>   pile = urwid.Pile(\[butten, other_butten\])
>
>   container = urwid.Filler(butten)
>
>   root = urwid.MainLoop(container)
>
>   root.run()


Okay, so now we have two buttons! You also may notice that you get a bonus here: you can change focus between the two buttons using the up and down arrow keys. This is a nice touch, I'll admit. There is a catch here, though, and it comes in the form of the Pile.  The Pile class is used to stack up widgets, and manage focus between them. It is what urwid calls a 'container widget', which does exactly what it sounds like: contains other widgets, gives them focus, and formats them into more useful UI components. 

Let's try another experiment with this code snippet before moving on. Get rid of the Filler widget, and feed the pile straight into the main loop. This will give you an error. This is because the pile is not a box widget. The function of the filler in the above listing is to give concrete dimensions to its contents.

## Box Widgets

   Box widgets are going to be a bit more familiar to those of us coming from some version of curses. A box widget has an explicit size, both vertically and horizontally. The aforementioned 'top' widget must be a box widget. This is why we needed to wrap our Text in a Filler during our hello world example. Let's play with box widgets for a moment.

>   import urwid
>
>   box = urwid.SolidFill("/")
>
>   root = urwid.MainLoop(box)
>
>   root.run()

That's all for now...There's much, much more to urwid, but that's enough for now. Next time we'll experiment with more complex layouts, and some of the other mechanisms that urwid provides for building robust text-based interfaces.

