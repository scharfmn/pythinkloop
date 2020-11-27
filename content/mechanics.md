Title: The mechanics of what's here
Date: 2020-11-27 20:15
Category: Meta

Decided, after a long flirt with js-based solutions like [ghost](https://ghost.org/) (too much) and cool-looking but seemingly unmaintained things like [toto](https://github.com/cloudhead/toto) (Ruby, for one thing -- just don't know it) to go with [Pelican](https://blog.getpelican.com/) for this (very) static site. It's Python, it's battle-tested, and it's simple to host via a cloud bucket. (And that "Pelican" turns out to be an anagram of _calepin_, which is French for "notebook," made it that much nicer.)

Which cloud bucket, you ask? Have ended up on Google Cloud Platform for my personal projects. The reason is [Firebase](https://firebase.google.com/) (which was first recommended to me by my [Sensei](https://blog.samibadawi.com/). There doesn't seem to be an easier way to host a site when you're writing a few pages by hand etc. There is [GitHub Pages](https://pages.github.com/), which actually was easier until I found out about [the dark side of io](https://gigaom.com/2014/06/30/the-dark-side-of-io-how-the-u-k-is-making-web-domain-profits-from-a-shady-cold-war-land-deal/). So when I thought about this site, it was [Google Domains](https://domains.google.com) and [Google Cloud Storage](https://cloud.google.com/storage) that came to mind.

For [my first post](https://pythinkloop.com/hello-and-what.html), I read the [Pelican Quickstart](https://docs.getpelican.com/en/latest/quickstart.html), and followed the directions. My markdown is not particularly good, but there are cheat sheets. (I won't post one because I haven't settled on any.)

To host the site, I bought the domain and slowly followed the directions [here](https://cloud.google.com/storage/docs/hosting-static-website). Once I had completed all the steps, I uploadled the _contents_ of the entire `output` folder and all its subfolders from Pelican to the bucket via the bucket's UI -- that is, not `output` itself as parent; `yourbucket` _takes the place_ of `output`-as-folder. And then I tried it, and the site was there automagically.

So to publish this post, will take an incremental step toward CI by using Google Cloud's version of [rsync](https://cloud.google.com/storage/docs/gsutil/commands/rsync). From [this post](https://cloud.google.com/community/tutorials/automated-publishing-cloud-build), I learned the following:
"The -m flag accelerates upload by processing multiple files in parallel, and the -c flag avoids re-uploading unchanged files." The -r flag recurses through the subdirs. The -d flag, the doc says, "provides a means of making the contents of a destination bucket or directory match those of a source bucket or directory."

So the final command, having installed the [gsutil tool](https://cloud.google.com/storage/docs/gsutil) is: `gsutil -m rsync -r -d ./your-dir gs://your-bucket`

and if it works, this should show up...

(it worked after I did [this](https://stackoverflow.com/a/56952730/1599229) within the env)
