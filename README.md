# Powerline - colour & style for the Emacs modeline

  This is a fork of the [milkypostman powerline](https://github.com/milkypostman/powerline) project, with the following changes:

  - jr0cket theme with Emacs Live style colour theme
  - additional face to seperate the modeline sections further

  See my blog posts on [tweaking the Emacs modeline with Powerline](http://jr0cket.co.uk/2015/01/tweaking-emacs-modeline-with-powerline.html) and [create a custom them for Emacs Powerline](http://jr0cket.co.uk/2015/01/custom-powerline-theme-for-Emacs-modeline.html) for more details and to see examples of the customised modeline.

> The milkypostman project was a proposed version 2.0 of the original [Emacs Powerline](http://www.emacswiki.org/emacs/PowerLine) which is a fork of [vim-powerline](https://github.com/Lokaltog/vim-powerline).


## Installation

      (require 'powerline)
      (powerline-jr0cket-theme)

  The second line customizes `mode-line-format` according to the jr0cket theme.

  There are two other themes: `powerline-default-theme` and `powerline-center-theme`.

  You can revert back to the original value of `mode-line-format` that was being used when powerline was loaded using `powerline-revert`.

## Faces

The faces that powerline uses for the builtin themes are `powerline-active0`, `powerline-active1` and `powerline-active2` for the active modeline, and `powerline-inactive0`, `powerline-inactive1` and `powerline-inactive2` for the inactive modelines. You can add as many faces as you want and pass those faces to the corresponding `powerline-*` functions when creating your `mode-line-format`.


## Custom Themes

Please look over the `powerline-default-theme` and `powerline-center-theme` in [`powerline-themes.el`](https://github.com/milkypostman/powerline/blob/master/powerline-themes.el) for examples of themes that involve different justifications of modeline text.

You can write your own powerline theme by simply setting your own `mode-line-format` to be an evaluation (`:eval`) of the powerline functions. Notice in `powerline-default-theme` the `let*` defines two lists: `lhs` and `rhs` which are exactly the lists that define what goes on the left and right sides of the modeline. The `powerline-center-theme` demonstrates how to *center* justify part of the modeline and defines an additional `center` list which is exactly the modeline components to be displayed in the middle section.

In *most* circumstances you should only need to modify the builtin themes unless you are trying to do a particularly unique layout.


### Explanation

This theme does some tricks to improve performance and get all the text justified properly. First, it sets `lhs` and `rhs` to a list of powerline sections. You can easily re-utilize builtin modeline formatting by adding it as a raw powerline section. For example,

    (powerline-raw mode-line-mule-info nil 'l)
    
would add the formatting defined in `mode-line-mule-info` to the modeline as it appears in the default modeline.

The last line of this is what actually puts it all together, by concatonating the `lhs`, some "fill" space, and `rhs`.  This *must* be done to ensure that the padding in between the left and right sections properly fills the modeline.



## Improvements from this rewrite:

* Cleaner code.
* Try to simply be a *library* that provides functions for generating a mode-line
* Make right-aligned text actually be flush against the right side.
* Separators are designed to dynamically size their height based on the font settings.
* Separators spread their width to the nearest character width.  (This is required to make right-aligned text actually be right-aligned)


## Implementing New Separators

The function should return an XPM image created using the `create-image` function.

There is a function called `memoize` that will help make calling the function multiple times with the same parameters be much quicker by caching the return value.

Each divider should have the signature: `(face1 face2 &optional height)`

`face1` : the left-hand face

`face2` : the right-hand face

`height` : specifies the height of the XPM, most of time this is `(font-char-height)`

Separators should consider the `height` when they are created so that the mode-line can change sizes based on the font height.

