---
layout: post
title:  "How to fix your Python virtualenv after a Homebrew Python upgrade"
date:   2015-03-02
comments: true
categories: python osx homebrew
---

Do you manage your OS X Python installation with Homebrew? Have you recently upgraded Python? 

In my case it was a Python 3 point release; from 3.4.2 to 3.4.3. The downside is that this is enough to invalidate virtualenv's symbolic links.

### Example

    ~: cd ~/src/my_app
    ~/src/my_app: source venv/bin/activate
    [venv] ~/src/my_app: python
    dyld: Library not loaded: @executable_path/../.Python
      Referenced from: /Users/jeremycade/src/my_app/env/bin/python
      Reason: image not found
    Trace/BPT trap: 5

As I've mentioned the virtualenv symlinks to your Homebrew python installation no longer link to the correct locations. The solution is to delete, and regenerate virtualenv symlinks.

First thing, we need to ensure that the your virtualenv is not active.

    [venv] ~/src/my_app: deactivate

Next, delete the offending symlinks. 

    ~/src/my_app: find venv -type l -delete

In this case I've made use of the BSD [find](http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/find.1?query=find) command that ships with OS X.

The final step is to recreate your virtualenv. 

    ~src/my_app: virtualenv venv

