Title: Update: Hosting Pelican with Firebase
Date: 2020-12-28 19:15
Modified: 2020-12-29 13:23
Category: Meta
Slug: hosting-update
Keywords: hosting, platforms, providers
Author: Michael Scharf
Summary: What is powering this site?
Lang: en
Translation: false
Status: published

So I got a hosting bill for $18.33 US for December and... no. There's no traffic to this site, so I must be paying 60 cents a day for the load balancer part of [this tutorial](https://cloud.google.com/storage/docs/hosting-static-website). So I deleted the load balancer and thought again: Firebase.

So searched around and found [this tutorial](https://www.smashingmagazine.com/2020/04/free-developer-blog-hugo-firebase/) for Hugo + Firebase (by [Zara Cooper](https://twitter.com/Zara__Cooper) in Smashing Magazine) and I figured: it will probably work if I skip to the part where Hugo has already generated the site and then sub in the Pelican-generated site for the rest of the steps.

Recall [how I generated this Pelican site](https://pythinkloop.com/site-mechanics.html), but disregard all of the Google Cloud Storage bits.

With the site still generated as-was, I tried beginning from from the point of "Deploying Your Blog To Firebase" in [the Hugo-based tutorial](https://www.smashingmagazine.com/2020/04/free-developer-blog-hugo-firebase/) and... it did work. Here's what I did:

You need to init the project as a Firebase project in the main blog project directory. After the init the command comes a series of prompts, including an ask re: which directory to set as the "public directory." I took this to be idenitical with the "output" directory of Pelican, which in my case is:

    /home/mike/things/pythinkloop/output

But since we are initting in the main project directory, and looking at the Hugo tutorial example too, it looks like that an absolute path isn't needed -- just the relative one, in this case just the child `output` directory. (Note: almost certainly doesn't matter, but that's a fake dir structure.) When asked, I also added GitHub integration as well (so that every push to main should deploy automatically). The whole dialogue is included in Zara Cooper's excellent set of screenshots included in the tutorial, but here's a bit more, plus the GitHub integration piece:

    $ firebase init

    ? What do you want to use as your public directory? output
    ? Configure as a single-page app (rewrite all urls to /index.html)? No
    ? Set up automatic builds and deploys with GitHub? Yes
    ✔  Wrote output/404.html
    ? File output/index.html already exists. Overwrite? No
    i  Skipping write of output/index.html

    i  Detected a .git folder at /home/mike/things/pythinkloop
    i  Authorizing with GitHub to upload your service account to a GitHub repository's secrets store.

    Visit this URL on this device to log in:
    https://github.com/login/oauth/authorize?client_id=[big long id string]

    Waiting for authentication...

    ✔  Success! Logged into GitHub as scharfmn

    ? For which GitHub repository would you like to set up a GitHub workflow? (format: u
    ser/repository) scharfmn/pythinkloop

    ✔  Created service account github-action-9000000 with Firebase Hosting admin permissions.
    ✔  Uploaded service account JSON to GitHub as secret [secret].
    i  You can manage your secrets at https://github.com/scharfmn/pythinkloop/settings/secrets.

    ? Set up the workflow to run a build script before every deploy? No

    ✔  Created workflow file /home/mike/things/pythinkloop/.github/workflows/firebase-hosting-pull-request.yml
    ? Set up automatic deployment to your site's live channel when a PR is merged? Yes
    ? What is the name of the GitHub branch associated with your site's live channel? ma
    in

    ✔  Created workflow file /home/mike/things/pythinkloop/.github/workflows/firebase-hosting-merge.yml

    i  Action required: Visit this URL to revoke authorization for the Firebase CLI GitHub OAuth App:
    https://github.com/settings/connections/applications/[revoke-page-id]
    i  Action required: Push any new workflow file(s) to your repo

    i  Writing configuration info to firebase.json...
    i  Writing project information to .firebaserc...

    ✔  Firebase initialization complete!

Then I deployed manually:

    $ firebase deploy

    === Deploying to 'pythinkloop'...

    i  deploying hosting
    i  hosting[pythinkloop]: beginning deploy...
    i  hosting[pythinkloop]: found 47 files in output
    ✔  hosting[pythinkloop]: file upload complete
    i  hosting[pythinkloop]: finalizing version...
    ✔  hosting[pythinkloop]: version finalized
    i  hosting[pythinkloop]: releasing new version...
    ✔  hosting[pythinkloop]: release complete

    ✔  Deploy complete!

    Project Console: https://console.firebase.google.com/project/projectname/overview
    Hosting URL: https://pythinkloop.web.app

Everything was there, but deploy defaulted to the firebase "Hosting URL": https://pythinkloop.web.app

I needed to follow Firebase's instructions for [connecting up a "custom domain"](https://firebase.google.com/docs/hosting/custom-domain). This proved straight-forward.

Then I went over to GitHub and revoked access to the Firebase CLI as suggested:

    i  Action required: Visit this URL to revoke authorization for the Firebase CLI GitHub OAuth App:
    https://github.com/settings/connections/applications/[revoke-page-id]

The dialogue also confirmed that my next push should deploy this post:

    i  Action required: Push any new workflow file(s) to your repo

It's been awhile, so I need to look at the [Pelican quickstart](https://docs.getpelican.com/en/latest/quickstart.html) again first. Hopefully I'll be posting again before another month passes.
