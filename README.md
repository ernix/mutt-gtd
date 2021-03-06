mutt-gtd
========

Little [mutt](http://www.mutt.org) hack to toggle [gtd](http://de.wikipedia.org/wiki/Getting_Things_Done) tags via the custom email header field X-Label

Motivation and idea
-------------------

After reading [this](http://www.nikilster.com/thoughts/how-to-win-at-email/#sthash.PvTgsQ0I.dpbs) (via [@felixge](https://twitter.com/felixge)) I started using [this](http://blitiri.com.ar/p/other/mutt-labels/) but disliked the amount of required keystrokes to actually do the tagging of single emails.

Wouldn't it be easier to simple toggle a fixed set of tags? Currently I only use "urgent", "todo", "review" and "waiting" for my email gtd.

Here is the result
------------------

Put the script x-label-toggler in your `~/bin` directory and modify `~/.muttrc` in the follwing way. (Note: If you place it into another directory than `~/bin`, modify the folling configuration accordingly)

Map the keystrokes to the toggling script
-----------------------------------------

```
macro index <esc>w "<enter-command> set my_editor=\$editor<enter><enter-command> set editor=\"x-label-toggler waiting\"<enter><edit><enter-command> set editor=\$my_editor<enter>" "Toggle label waiting"
macro index <esc>t "<enter-command> set my_editor=\$editor<enter><enter-command> set editor=\"x-label-toggler todo\"<enter><edit><enter-command> set editor=\$my_editor<enter>" "Toggle label todo"
macro index <esc>u "<enter-command> set my_editor=\$editor<enter><enter-command> set editor=\"x-label-toggler urgent\"<enter><edit><enter-command> set editor=\$my_editor<enter>" "Toggle label urgent"
macro index <esc>r "<enter-command> set my_editor=\$editor<enter><enter-command> set editor=\"x-label-toggler review\"<enter><edit><enter-command> set editor=\$my_editor<enter>" "Toggle label review"
macro pager <esc>w "<enter-command> set my_editor=\$editor<enter><enter-command> set editor=\"x-label-toggler waiting\"<enter><edit><enter-command> set editor=\$my_editor<enter><clear-flag>N<enter>" "Toggle label waiting"
macro pager <esc>t "<enter-command> set my_editor=\$editor<enter><enter-command> set editor=\"x-label-toggler todo\"<enter><edit><enter-command> set editor=\$my_editor<enter><clear-flag>N<enter>" "Toggle label todo"
macro pager <esc>u "<enter-command> set my_editor=\$editor<enter><enter-command> set editor=\"x-label-toggler urgent\"<enter><edit><enter-command> set editor=\$my_editor<enter><clear-flag>N<enter>" "Toggle label urgent"
macro pager <esc>r "<enter-command> set my_editor=\$editor<enter><enter-command> set editor=\"x-label-toggler review\"<enter><edit><enter-command> set editor=\$my_editor<enter><clear-flag>N<enter>" "Toggle label review"

macro index <esc>lw "<limit>~y waiting!~D<enter>" "Limit view to tag waiting"
macro index <esc>lt "<limit>~y todo!~D<enter>" "Limit view to tag todo"
macro index <esc>lu "<limit>~y urgent!~D<enter>" "Limit view to tag urgent"
macro index <esc>lr "<limit>~y review!~D<enter>" "Limit view to tag review"
macro index <esc>ld "<limit>!(~y review|~y urgent|~y waiting|~y todo)!~D<enter>" "Limit view to all untagged messages"
```

Enable coloring in mutt
-----------------------

```
color index green default '~y waiting'
color index yellow default '~y review'
color index brightred default '~y todo'
color index brightred default '~y urgent'
color header default green '^X-Label: waiting'
color header default yellow '^X-Label: review'
color header default brightred '^X-Label: todo'
color header default brightred '^X-Label: urgent'
```

Enable display of X-Label
-------------------------

```
set index_format="%4C %Z %{%b %d} %-15.15L %?M?(#%03M)&(%4l)? %?y?(%.20Y) ?%s"
```

How to use it
-------------

In mutt you hit `<esc>w` to mark an email with the tag "waiting" and unmark it by hitting `<esc>w` again. To limit your view to emails with tag "waiting" just hit `<esc>lw`. Use `u` for "urgent", `t` for "todo" and `r` for "review". With `<esc>ld` you limit your view to all email threads that have no tags assigned.


Happy tagging
