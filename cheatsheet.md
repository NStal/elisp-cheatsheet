## General

running a repl

    M-x ieml

writing a script
```bash
#!/usr/local/bin/emacs --script
```

compiling

    M-x byte-compile-file

## Elisp Stuff

lexical lets (from CL, and not really used)

```lisp
(lexical-let ((x 5)
              (y 10))
  (defun get-stuff ()
    (list x y)))

(let ((x 10)
      (y 20))
  (get-stuff)) ;; => (5 10) :D however never releases variables :(
```

new in emacs 24, you can do this in your file header:

```lisp
;;; -*- lexical-binding: t -*-
```

and it will use lexical bindings!! yay.

### current cusor

```lisp
(point) ;; cursor position

(region-beginning) ;; start/end of region
(region-end)

(point-min)
(point-max) ;; end of (possibly narrowed) buffer
```

### moving cursor

```lisp
(goto-char 100) ;; move to position

(forward-char 10) ;; forward/backward chars
(backward-char 10)

(skip-chars-forward "chars") ;; skip given chars
(skip-chars-backward "chars") ;; skip given chars back

(search-forward "foo") ;; search for foo, move cursor there
(search-backward "foo")
(search-forward-regexp "blah")
(search-backward-regexp "blah")

(beginning-of-line)
(end-of-line)
```

### editing text

```lisp
(insert "foo" "bar")
(insert-buffer-substring-no-properties buffer start end)
(insert-file-contents "cheatsheet.el")
```

### deleting text

```lisp
(delete-char 10)
(delete-region start end)
(erase-buffer)
```

### capitalization

```lisp
(upcase-word 20) ;; next 20 words
(upcase-region start end)

(upcase-initials "foo bar")
(capitalize-word 10)
(capitalize-region start end)

(downcase-word 10)
(downcase-region start end)
```

### find and replace

```lisp
;; string replace in buffer
(let ((case-fold-search t))
  (goto-char (point-min))
  (while (search-forward "to-replace" nil t) ;; could use search-forward-regexp
    (replace-match "with-this" t t)))

(match-string 5) ;; nth text matched by last search
(match-beginning 5) (match-end 5) ;; start/end of nth matched
```

### grabbing text

```lisp
(buffer-substring start end) ;; grab string with face info
(buffer-substring-no-properties start end)

(thing-at-point 'word) ;; 'word 'symbol 'line 'sentence 'filename 'URL etc
(bounds-of-thing-at-point 'line) ;; cons pair of start/end
```

### strings (not many, usually use a temp buffer - with-temp-buffer)

```lisp
(length "123")
(substring "long long long string" 5 10)
(concat "foo" "bar")
(string-match "(bunch)" "a word bunch of words")
(match-string 0 "a word bunch of words")
(split-string "foo-bar-bam" "-")
(string-to-number "4")
(number-to-string 4)
(format "%d" 5)
```

example of normal text processing

```lisp
(with-temp-buffer
  (insert "here is some text and some more text")
  (goto-char (point-min))
  (while (search-forward "some" nil t)
    (replace-match "BLAH" t t))
  (buffer-string))
```

preserving point, mark, buffer, narrow

```lisp
(save-excursion
  ;; gonna move the cursor around but it will go back after
  ;; exiting this macro
  )

(save-restriction
  ;; preserver users narror-to-region before you do any narrowing
  )
```

### buffer functions

```lisp
(buffer-name) ;; name of current
(buffer-file-name)
(set-buffer "*scratch*")
(kill-buffer "*Help*")

;; like example above
(with-temp-buffer
  (insert "some shit")
  (buffer-string))
```

### environment / files

```lisp
(shell-command "ls -la") ;; outputs to buffer
command-line-args
(getenv "HOME") ;; environment variables

(find-file "filepath") ;; opens buffer
(save-buffer) ;; writes to file
(write-file "foo") ;; save-as
(append-to-file start end "foo")
(kill-buffer "name")

(rename-file "from" "to")
(copy-file "from" "to")
(delete-file "name")
(copy-directory "from" "to")
(delete-directory "name")

(file-name-directory "foo/bar/bam.zip") ;; => foo/bar/
(file-name-nondirectory "foo/bar/bam.zip") ;; => bam.zip
(file-name-extension "foo/bar/bam.zip") ;; => zip
(file-name-sans-extension "foo/bar/bam.zip") ;; => foo/bar/bam
(file-relative-name "/foo/bar/bam.zip") ;; => ../../../../foo/bar/bam.zip
(expand-file-name "bam.zip") ;; => "/Users/gjones/sandbox/elisp-cheatsheet/bam.zip"
```













