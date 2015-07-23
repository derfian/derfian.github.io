---
title: Quickly move mails to a preset folder with mu4e
layout: post
---

[mu4e](http://www.djcbsoftware.nl/code/mu/mu4e.html) has been my
primary mail client a few years now and I've been mostly happy. Since
spam is a reality when it comes to email, I'd like to be able to
easily move spam my spamfilter doesn't catch to a separate Maildir
folder, so I can train my spamfilter to catch it in the future.

Moving mails in `mu4e` is as easy as pressing `m` - you'll be prompted
for the destination. You could also mark several mails with `*` and
move them all at once, but you still need to enter the destination
folder. It would be much better if I could just press a key and have
the mails marked for move into the right folder, just as easy as
refiling or marking mails as read.

Here's one way to do just that:

{% highlight emacs-lisp %}
(defun derfian/mu4e-headers-learn-spam ()
  (interactive)
  (mu4e-mark-set 'move "/INBOX.Learn.SPAM")
  (mu4e-headers-next))

(defun derfian/mu4e-view-learn-spam ()
  (interactive)
  (mu4e~view-in-headers-context
   (derfian/mu4e-headers-learn-spam)))
{% endhighlight %}

I found that `l` was an unused key in both modes:

{% highlight emacs-lisp %}
(define-key 'mu4e-headers-mode-map (kbd "l")
            #'derfian/mu4e-headers-learn-spam)
(define-key 'mu4e-view-mode-map (kbd "l")
            #'derfian/mu4e-view-learn-spam)
{% endhighlight %}

Now I only have to press `l` in the headers or message view, and the
mail will be marked to be moved to the right folder and move me to the
next mail. Perfect!
