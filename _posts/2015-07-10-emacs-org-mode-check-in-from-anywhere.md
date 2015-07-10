---
title: org-clock-in from any Emacs buffer
layout: post
---

While I track my work time in org-mode documents, I rarely spend all
my time in the actual document. I spend more time in other buffers,
editing code or writing documentation, or even in terminals outside of
Emacs.

So when I move from working on development and move on to answering a
support issue, I would have to go to my org buffer, navigate to the
right headline and then finally `C-c C-x C-i`. This is somewhat of a
pain, so I started reading up on how to make this workflow easier.

I came up with a solution that looks like this:

{% highlight emacs-lisp %}
;; Org-goto headline, clock in and return to original position
(defun derfian/org-goto-and-clock-in ()
  (interactive)
  (window-configuration-to-register :derfian/work-checkin)
  (save-excursion
    (find-file (expand-file-name "~/Dropbox/Work.org.gpg"))
    (org-goto)
    (org-clock-in))
  (jump-to-register :derfian/work-checkin))
{% endhighlight %}

I assigned this to a key with
`(global-set-key (kbd "<f7>") #'derfian/org-goto-and-clock-in)`, and
now I only have to press F7 to be prompted for which headline
to check in to. Very nice.
