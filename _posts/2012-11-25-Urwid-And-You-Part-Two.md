---
layout: post
category : urwid
tags : []
published: false
---
{% include JB/setup %}

So last time we discussed some of the basics of using Urwid, including the obligatory hello world example and a couple of peeks at how Urwid encourages us to think about laying out our UI. Before we start discussing how to tie application logic into an urwid application, let's get a bit deeper into the way this library encourages us to think, so as to avoid frustration down the road.

If you're familiar with event-driven UI frameworks such as python's tkinter module, you won't be terribly alienated.

We've already seen that Urwid can package several small widgets for us into something useful such as a series of buttons while managing focus for us. Let's see what happens when we start making our use case a bit more complex.

> import urwid
> 
> butten = urwid.Button("Release the Kraken!")
> other_butten = urwid.Button("Pew Pew Lasers!")
> last_butten = urwid.Button("Anti-Yeti Missiles")
> 
> button_pile = urwid.Pile(\[butten, other_butten, last_butten\])
> 
> checkbox = urwid.CheckBox("Self Destruct?")
> other_box = urwid.CheckBox("Post to facebook?")
> final_box = urwid.CheckBox("Checkbox?")
> 
> box_pile = urwid.Pile(\[checkbox, other_box, final_box\]) 
> 
> pile_pile = urwid.Pile(\[button_pile, box_pile\])
> 
> container = urwid.Filler(pile_pile)
> 
> root = urwid.MainLoop(container)
> root.run()

Okay, so once again Urwid is smart enough to handle focus for us. Let's say we want our two UI components (checkbox and button list) to be side-by-side rather than vertically stacked. Let's make the following small modification to our program:


> import urwid
> 
> butten = urwid.Button("Release the Kraken!")
> other_butten = urwid.Button("Pew Pew Lasers!")
> last_butten = urwid.Button("Anti-Yeti Missiles")
> 
> button_pile = urwid.Pile(\[butten, other_butten, last_butten\])
> 
> checkbox = urwid.CheckBox("Self Destruct?")
> other_box = urwid.CheckBox("Post to facebook?")
> final_box = urwid.CheckBox("Checkbox?")
> 
> box_pile = urwid.Pile(\[checkbox, other_box, final_box\]) 
> 
> pile_columns = urwid.Columns(\[button_pile, box_pile\])
> 
> container = urwid.Filler(pile_columns)
> 
> root = urwid.MainLoop(container)
> root.run()

As you can see, the Columns widget is a horizontally-stacked analog of the Pile, and it works in quite the same fashion. Let's do a bit more tinkering and then we'll move into some more interesting territory.

First let's add some lovely borders around our menus. Make the following slight modification:

> import urwid
> 
> butten = urwid.Button("Release the Kraken!")
> other_butten = urwid.Button("Pew Pew Lasers!")
> last_butten = urwid.Button("Anti-Yeti Missiles")
> 
> button_pile = urwid.Pile(\[butten, other_butten, last_butten\])
> button_box = urwid.LineBox(button_pile)
> 
> checkbox = urwid.CheckBox("Self Destruct?")
> other_box = urwid.CheckBox("Post to facebook?")
> final_box = urwid.CheckBox("Checkbox?")
> 
> box_pile = urwid.Pile(\[checkbox, other_box, final_box\]) 
> box_box = urwid.LineBox(box_pile)
> 
> pile_columns = urwid.Columns(\[button_box, box_box\])
> pile_columns_box = urwid.LineBox(pile_columns)
> 
> container = urwid.Filler(pile_columns_box)
> 
> root = urwid.MainLoop(container)
> root.run()

This code is getting quite ugly, isn't it? Don't worry, we'll refactor very soon, but first run this listing.

Ooh, pretty. Very nice touch, those boxes. The LineBox class is not technically a container class, even though it seems intuitive to think of it in that way. It's what Urwid calls a _Decorator Widget_, and modifies the appearance of the widget or widgets it contains. There are more examples of these to come, but for now just know that when you want to dress up your widgets with color or borders, you'll be wrapping them in widgets. 

I'll admit our examples thus far have been very boring, but I wanted to illustrate that in Urwid, widgets are not just UI elements, they're the logical unit Urwid works with. The idea of coloring a widget by wrapping it in a widget seemed counter-intuitive to me at first, but once you get used to it, it ain't so bad.

# Doing Stuff With Your Widgets

Alright let's flesh this puppy out so that we can get some interactivity here. Today's last listing ties some logic into our UI so that we can actually make our buttons and checkboxes DO something. 

> import urwid
> 
> status = ""
> 
> def update_status_text():
>     status_text.set_text(status)
> 
> def post_to_facebook():
>     global status
>     status += "Activity Posted!"
> 
> def self_destruct():
>     global status
>     status += "3...2...1..."
>     quit
> 
> def evaluate_options():
>     if checkbox.state == True:
>         self_destruct()
>     if other_box.state == True:
>         post_to_facebook()
> 
> def fire_lasers(data):
>     global status
>     status += "Pew Pew!"
>     evaluate_options()
>     update_status_text()    
> 
> def fire_missiles(data):
>     global status
>     status += "Whooosh!"
>     evaluate_options()
>     update_status_text()
> 
> def release_the_kraken(data):
>     global status
>     status += "God save us for what we have done..."
>     evaluate_options()
>     update_status_text()
> 
> butten = urwid.Button("Release the Kraken!", on_press=release_the_kraken)
> other_butten = urwid.Button("Pew Pew Lasers!", on_press=fire_lasers)
> last_butten = urwid.Button("Anti-Yeti Missiles", on_press=fire_missiles)
> 
> button_pile = urwid.Pile(\[butten, other_butten, last_butten\])
> button_box = urwid.LineBox(button_pile)
> 
> checkbox = urwid.CheckBox("Self Destruct?")
> other_box = urwid.CheckBox("Post to facebook?")
> final_box = urwid.CheckBox("Checkbox?")
> 
> box_pile = urwid.Pile(\[checkbox, other_box, final_box\]) 
> box_box = urwid.LineBox(box_pile)
> 
> pile_columns = urwid.Columns(\[button_box, box_box\])
> pile_columns_box = urwid.LineBox(pile_columns)
> 
> status_text = urwid.Text(status)
> status_box = urwid.LineBox(status_text)
> 
> top_level_pile = urwid.Pile(\[pile_columns_box, status_box\])
> 
> container = urwid.Filler(top_level_pile)
> 
> root = urwid.MainLoop(container)
> root.run()

As you can see, Urwid allows us to tie callbacks into our widgets so we can execute code in an event-based manner. Fantastic, don't you think? My crude example illustrates this by adding a 'status' text-display area to the program, and updating it every time we click a button. As you can see, each time a button is clicked, a callback is executed.

These callbacks take a single argument passed to them from urwid containing data on the context of its execution, but we'll leave that for another time.  Also illustrated here is the manner in which we are able to update the contents of a text widget.

That's all for now. Next time we'll refactor this horrificly redundant code into something a tiny bit more tolerable, and we'll move on into more exciting territory, including colors. Till then, please let me know of any errors I've overlooked, or if you have any suggestions for future tutorials.