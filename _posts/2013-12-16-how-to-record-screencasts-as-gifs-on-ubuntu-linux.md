---
layout: post
title: "How to record screencasts as GIFs on Ubuntu/Linux"
permalink: /blog/2/how-to-record-screencasts-as-gifs-on-ubuntu-linux
---

A screencast says more than a thousand pictures. That's why I like to record
my screen as a GIF when I want to demonstrate a new feature or experiment I'm working on.

The core of the method consists of using Byzanz to record your screen
as a GIF. First, to install it, you should run:

{% highlight bash %}
sudo add-apt-repository -y ppa:fossfreedom/byzanz
sudo apt-get update
sudo apt-get install -y byzanz
{% endhighlight %}

Then, you can use this command to save a recording of a portion of your
screen to a file in the current directory:

{% highlight bash %}
byzanz-record --duration=5 --x=100 --y=100 --width=400 --height=300 screencast.gif
{% endhighlight %}

Improving the method
---

Two problems arise: how to know the coordinates you want to use for the
recording, and how to know how much time is left.

To get information about the screen coordinates, you could use xdotool to
track your mouse position. First, install it (`sudo apt-get install xdotool`).
Then, run this snippet:

{% highlight bash %}
while true; do xdotool getmouselocation; sleep 0.1; clear; done
{% endhighlight %}

Now this terminal screen will show live information about your mouse position.
Use this to get your x, y, width and height parameters for byzanz-record.

This bash function displays a running countdown:

{% highlight bash %}
countdown()
(
  IFS=:
  set -- $*
  secs=$(( ${1#0} * 3600 + ${2#0} * 60 + ${3#0} ))
  while [ $secs -gt 0 ]
  do
    sleep 1 &
    printf "\r%02d:%02d:%02d" $((secs/3600)) $(( (secs/60)%60)) $((secs%60))
    secs=$(( $secs - 1 ))
    wait
  done
  echo
)
{% endhighlight %}

By using it along the byzanz-record command, we can have a live countdown of the
time remaining on our recording. The `sleep 1` part is important because the recording
tool has a default delay of one second before starting. (You can use the `--delay=X` parameter to modify this).

{% highlight bash %}
byzanz-record --duration=5 --x=100 --y=100 --width=400 --height=300 screencast.gif & sleep 1; countdown "00:00:05"
{% endhighlight %}

Naturally, after the terminal loses focus you will not be able to see the timer anymore.
However, if you set this terminal window to always be on top and move it away from the
action, you can go along your demonstration and still keep track of the time remaining.

![A Screencast](http://i.imgur.com/5gNiyFs.gif)
