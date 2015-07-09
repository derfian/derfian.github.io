---
title: Toggle "fullscreen applications" in Emacs
layout: post
---

The downside of being an avid Emacs user is that I always have a lot
of buffers open, and I find Emacs window management is somewhat
cumbersone. Because of that, I have hotkeys to read mail and open my
Org-mode file for work notes so I wouldn't have to go hunt for them.

Hotkeys worked fine for launching my mail reader and so on, but being
able to hide them again is equally important to me. This is how I
solved it - one press of a button and mu4e will appear. Press it again
and your previous windows will show up.

{% highlight emacs-lisp %}
(global-set-key (kbd "<f6>") #'derfian/toggle-mu4e)

(defun derfian/toggle-mu4e ()
  "Launch, open or close mu4e."
  (interactive)
  (let
    ((current-mode
      (buffer-local-value 'major-mode (current-buffer))))

    ;; If looking at mu4e, restore the previous windows.
    (if (or (eq current-mode 'mu4e-main-mode)
            (eq current-mode 'mu4e-headers-mode)
            (eq current-mode 'mu4e-view-mode))
        (progn
          (window-configuration-to-register :derfian/mu4e-active)
          (jump-to-register :derfian/before-mu4e))

      (progn
        ;; Save current window configuration to register so we can
        ;; return later.
        (window-configuration-to-register :derfian/before-mu4e)

        (cond
         ;; There's a mu4e window register active - go
         ;; there. advices around mu4e and mu4e-quit makes sure to
         ;; set and unset this register as needed.
         ((get-register :derfian/mu4e-active)
          (jump-to-register :derfian/mu4e-active))

         ;; If there's no active mu4e window register, we'll have to
         ;; start mu4e ourselves. We can allow it to go fullscreen
         ;; by deleting all other windows. They're still saved to
         ;; the :derfian/before-mu4e register above.
         (t
          (progn (delete-other-windows)
                 (mu4e))))))))


(defadvice mu4e (before mu4e-activate-registers activate)
  "Saves the previous window configuration on mu4e"
  (window-configuration-to-register :derfian/before-mu4e))

(defadvice mu4e-quit (after mu4e-destroy-registers activate)
  "Restores the previous window configuration after mu4e-quit"
  (set-register :derfian/mu4e-active nil))

{% endhighlight %}


