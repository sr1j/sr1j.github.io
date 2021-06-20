---
title: My first open sorce contribution to SerenityOS"
author: Sreelakshmi Jayarajan
date: 2021-06-20 20:55:00 +0800
categories: [OSS, SerenityOS]
tags: [getting started]
---

How can I contribute to open source software? Can I contribute with no prior development experience? Is it meant for beginners? If yes, is it very hard? These are some common questions many students find asking themselves. Well, I did, too. Hence I decided to share my experience contributing to SerenityOS [SerenityOS] — a graphical, Unix-like operating system for x86 computers — to help others contribute to Serenity or any large open source project.

Here’s a little bit about me. I’m Sreelakshmi, a first year undergraduate student from Kerala, India. I came to know about SerenityOS through my friend and what I found really exciting about it was the fact that it was built from the ground up! Moreover, the topic of operating systems and their functions intrigued me, so I decided to contribute there. I’m familiar with the basics of C++ from high school which was helpful since the codebase of SerenityOS is in C++. But I had no prior experience in C++ development.

SerenityOS actually started as a hobby project by [Andreas Kling], and as of June 2021, over 400 people have contributed to it and around 3,000 people are part of the SerenityOS Discord server! I’m really happy to be part of such an amazing community! 

## Getting started

The first and foremost thing I did was completely move from Windows to Ubuntu as Linux distributions are much better for programmers. I was also new to Git and GitHub, so I learned the basic concepts and commands on the go. I began the contributing process by forking and cloning the repository. The next challenge was building SerenityOS. I struggled with this part for a few days because of various reasons including running incorrect commands and getting confused between the build instructions for different systems. I eventually asked for some help at their Discord, and they suggested a different command for me to try after which I was able to build and run it successfully! The next challenge was finding an issue to work on.

## How I chose an issue and what was interesting about it

Many open source projects have beginner-friendly issues labelled “good-first-issue” or something similar. So I went to SerenityOS’ issue tracker on GitHub and went through all the  good first issues hoping to find something easy yet interesting. I found one called “GML Playground: Hovering over the auto-completion window makes it disappear” [Issue] and decided to go with it. So the autocomplete suggestions window in the GML Playground disappears when a user hovers over it, and I needed to fix that.

What was interesting about it? I found the same bug in SerenityOS’ HackStudio IDE and moreover I saw Andreas facing it in one his [Youtube Video].

But finding your first issue may be difficult; there are usually only a few good first issues, and there’s a good chance that they've already been taken up by someone else. However, one of the best parts of contributing to an open source project is that you can suggest your own ideas and/or find bugs after playing with different things in the project, i.e., you can open your own good first issues!

## Finding solutions

I tried to find the associated files and got to `LibGUI/Window` and `WindowServer/WindowManager` because the auto-completion window is a window. I went through the code in those files, used debug statements and played with some functions such as `hide()`, `show()`, `move_to_front_and_make_active()`, `process_mouse_event()`, etc.

Later on I found out that the auto-completion window was being closed inside the `leave_event()` of the `LibGUI/TextEditor`. So I tried adding some conditions prior closing it to check whether the last mouse pointer position was inside the auto-completion window’s `rect()` (using coordinates and dimensions of the auto-completion window) and only close it if it doesn't have the mouse pointer inside it. Unfortunately that didn’t work out because the `rect()` wasn’t behaving as I expected it to - the coordinates of the rectangle seemed to be a bit mispositioned.

I also tried out approaches like closing the auto-completion window only when the last mouse pointer position was not in the `rect()` of its parent widget (TextEditor). To do that, I played with all the `rect()` functions in `LibGUI/Widget` but none of them gave me the results I was looking for.

After a week, I finally decided to ask for some help at Discord. I put a text explaining all the solutions I tried and asked if they had any hints or suggestions. One of them suggested I just `show()` the auto-completion window itself when it gets a mouse enter event, but wasn't able to implement it successfully because of the aforementioned `rect()` issue.

On the issue, I also received a suggestion to only close the auto-completion window when the user completely switches to another window. But this solution doesn’t close the auto-completion window when the mouse pointer leaves the GML Playground window (which is what should ideally happen).

It took me about 2+ weeks to try all these approaches so I felt a bit sad and exhausted at times, but I also didn't feel like giving up. My friends suggested I take a break, so I kept the issue aside for a while.

## My first pull request to SerenityOS

After 3-4 days, I got back into it with a fresh mind and was able to find a function in `WindowServer/Window` which lets you iterate through all the child windows of a parent. I then went to the `set_active_window()` inside `WindowServer/WindowManager` and wrote some code to close all the child windows of WindowType `Tooltip` (which is the type of the auto-completion window) of the previously active window. Afterwards, I removed the code responsible for closing the window in `TextEditor`’s `leave_event()`, and it worked!

I went through the contribution guidelines of SerenityOS, committed my changes and opened a [PR]. A reviewer pointed out some of the mistakes with my commit messages so I had to squash commits after updating their description. The git commands were frustrating sometimes, but with the help of my friend, I managed to make the necessary changes. After all the CI checks passed, my PR was finally merged! 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">WindowManager: Close tooltip child_windows of previously active window<br><br>This closes the autocomplete_box used in applications like GML<br>Playground and HackStudio when the user switches focus to a new window.<br>Author: Sreelakshmi<a href="https://t.co/uCkOTtAGbb">https://t.co/uCkOTtAGbb</a></p>&mdash; SerenityOS Commits 🐞 (@OsCommits) <a href="https://twitter.com/OsCommits/status/1403795357209923590?ref_src=twsrc%5Etfw">June 12, 2021</a></blockquote> 
 
 
## Final thoughts

I’m really really happy that my first ever git commit was to SerenityOS! If you’re interested in contributing to SerenityOS but consider yourself a beginner, I would suggest you to not worry about your C++ development experience and just get started as soon as possible! Find and pick a good first issue, or try out different things (there are lots of fun Userland applications in SerenityOS!) and open your bug/enhancement issues. Don’t get overwhelmed by looking at the size of the project — you only need to understand a tiny part of the codebase first, then go up from there. You can always ask for help if you get stuck or need suggestions but make sure to do your best first. Make sure the questions are to the point and mention everything you’ve already tried so that they can help you better. Most importantly, **never give up cuz great things take time!!!**

Lastly, I would like to thank [Alimpfard], [Gunnar Beutner] and [Anand Baburajan] for their help, FOSS United’s [First Commit group] for their suggestions and Andreas Kling for creating Serenity and giving us an opportunity to be part of it.

Looking forward to learning and contributing more to SerenityOS!

**Thank you for reading! :^)**

[SerenityOS]: http://serenityos.org/
[Issue]: https://github.com/SerenityOS/serenity/issues/7165
[Andreas Kling]: https://awesomekling.github.io/about/
[Youtube Video]: https://youtu.be/O3MtPgTUOC8?t=1373
[PR]: https://github.com/SerenityOS/serenity/pull/7992
[Alimpfard]: https://github.com/alimpfard
[Gunnar Beutner]: https://github.com/gunnarbeutner
[Anand Baburajan]: https://github.com/anandbaburajan
[First Commit group]: https://fossunited.org/first-commit























