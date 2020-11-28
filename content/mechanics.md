Title: The mechanics of what's here
Date: 2020-11-27 20:15
Category: Meta

Which static-site generator to use? I looked at js-based solutions like [ghost](https://ghost.org/) (too much), and the great Go-based [Hugo](https://gohugo.io/) (maybe after I finally learn Go). In Ruby-world, there is the cool-looking but seemingly unmaintained [toto](https://github.com/cloudhead/toto) (last commit in 2013), and the major player: [Jekyll](https://jekyllrb.com/). 

Jekyll seems like the obvious choice: it is the force behind [GitHub Pages](https://pages.github.com/), which actually what I used for prior projects. And the amazing-looking [staticman](https://staticman.net/) is built for it -- it actually solves the still-giant problem of comments for static sites. 

The problem, for me, with the Jekyll-GitHub Pages-staticman setup is [the dark side of io](https://gigaom.com/2014/06/30/the-dark-side-of-io-how-the-u-k-is-making-web-domain-profits-from-a-shady-cold-war-land-deal/). Even if you are using your own domain with Github pages, it seemingly relies under the hood on a `.io` domain, which I don't want to engage.

And so, since I also do not particularly want to get involved with [Disqus](https://disqus.com/), I decided to forget about comments for now and go with [Pelican](https://blog.getpelican.com/). It's Python, it's battle-tested, and it's simple to host via a cloud bucket. And that "Pelican" turns out to be an anagram of _calepin_, which is French for "notebook," made it that much nicer.

Which cloud bucket, you ask? Have ended up on Google Cloud Platform for my personal projects. The reason has been, up to this point, [Firebase](https://firebase.google.com/) (which was first recommended to me by my [Sensei](https://blog.samibadawi.com/)). When you're writing a few pages by hand, and/or need a db, there doesn't seem to be an easier way to host a site. That's not what this is, though, so when I thought about this site, I thought: [Google Domains](https://domains.google.com) and [Google Cloud Storage](https://cloud.google.com/storage). Once I found [this tutorial for static site hosting](https://cloud.google.com/storage/docs/hosting-static-website), I decided to try it.

For [my first Pelican-based post](https://pythinkloop.com/hello-and-what.html), I read the [quickstart](https://docs.getpelican.com/en/latest/quickstart.html), and followed the directions. My markdown is not particularly fast, but there are cheat sheets. (I won't post one because I haven't settled on any.)

I bought the domain and slowly followed the hosting tutorial [directions](https://cloud.google.com/storage/docs/hosting-static-website). Once I had completed all the steps, I uploadled the _contents_ of the entire Pelican-generated `output` folder and all its subfolders to the cloud bucket via the bucket's UI -- that is, not `output` itself as parent; `yourbucketname` _takes the place_ of `output`-as-parent-folder. And then I tried it, and my hello was there automagically.

To publish this post, will take an incremental step toward CI by using Google Cloud's version of [rsync](https://cloud.google.com/storage/docs/gsutil/commands/rsync). From [this post](https://cloud.google.com/community/tutorials/automated-publishing-cloud-build), I learned the following:
"The -m flag accelerates upload by processing multiple files in parallel, and the -c flag avoids re-uploading unchanged files." The -r flag recurses through the subdirs. The -d flag, the doc says, "provides a means of making the contents of a destination bucket or directory match those of a source bucket or directory."

So the final command, having installed the [gsutil tool](https://cloud.google.com/storage/docs/gsutil) is: `gsutil -m rsync -r -d ./your-dir gs://your-bucket`

and if it works, this should show up...

(it worked after I did [this](https://stackoverflow.com/a/56952730/1599229) within the env)
