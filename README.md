# Clojure for Brave and True
Notes are based on the online book https://www.braveclojure.com/.

## 0 Environment
- You can check your version by running java -version in your terminal, and download the latest Java Runtime Environment (JRE) from http://www.oracle.com/technetwork/java/javase/downloads/index.html.
- Then, install Leiningen using the instructions on the Leiningen home page at http://leiningen.org/ (Windows users, note there’s a Windows installer). You can also use a package manager.
- When you install Leiningen, it automatically downloads the Clojure compiler, clojure.jar.

## 1 Noob Project
## 1.1 Creating a new Project
Type in terminal
```
lein new app clojure-noob
```
This command should create a directory structure that looks similar to this (it’s okay if there are some differences):
```
| .gitignore
| doc
| | intro.md
➊ | project.clj
| README.md
➋ | resources
| src
| | clojure_noob
➌ | | | core.clj
➍ | test
| | clojure_noob
| | | core_test.clj
```
This project skeleton isn’t inherently special or Clojure-y. It’s just a convention used by Leiningen. You’ll be using Leiningen to build and run Clojure apps, and Leiningen expects your app to have this structure. The first file of note is `project.clj` at ➊, which is a configuration file for Leiningen. It helps Leiningen answer such questions as “What dependencies does this project have?” and “When this Clojure program runs, what function should run first?” In general, you’ll save your source code in `src/<project_name>`. In this case, the file `src/clojure_noob/core.clj` at ➌ is where you’ll be writing your Clojure code for a while. The `test` directory at ➍ obviously contains tests, and resources at ➋ is where you store assets like images.

### 1.1 Running a Project
Now let’s actually run the project. Open `src/clojure_noob/core.clj` in your favorite editor. You should see this:
```clj
(ns clojure-noob.core
  (:gen-class))

➋ (defn -main
  "I don't do a whole lot...yet."
  [& args]
➌   (println "Hello, World!"))
```
The lines at ➊ declare a namespace, which you don’t need to worry about right now. The `-main` function at ➋ is the entry point to your program, a topic that is covered in Appendix A. For now, replace the text `"Hello, World!"` at ➌ with `"I'm a little teapot!"`. The full line should read `(println "I'm a little teapot!"))`.

Next, navigate to the clojure_noob directory in your terminal and enter:
```
lein run
```
You should see the output `"I'm a little teapot!"` Congratulations, little teapot, you wrote and executed a program!

You’ll learn more about what’s actually happening in the program as you read through the book, but for now all you need to know is that you created a function, `-main`, and that function runs when you execute `lein run` at the command line.

### 1.2 Building a Project
Using `lein run` is great for trying out your code, but what if you want to share your work with people who don’t have Leiningen installed? To do that, you can create a stand-alone file that anyone with Java installed (which is basically everyone) can execute. To create the file, run this:
```
lein uberjar
```
This command creates the file `./target/uberjar/clojure-noob-0.1.0-SNAPSHOT-standalone.jar`. You can make Java execute it by running this:
```
java -jar target/uberjar/clojure-noob-0.1.0-SNAPSHOT-standalone.jar
```
Look at that! The file `target/uberjar/clojure-noob-0.1.0-SNAPSHOT-standalone.jar` is your new, award-winning Clojure program, which you can distribute and run on almost any platform.

### 1.3 Using the REPL
The REPL is a tool for experimenting with code. It allows you to interact with a running program and quickly try out ideas. It does this by presenting you with a prompt where you can enter code. It then reads your input, evaluates it, prints the result, and loops, presenting you with a prompt again.

This process enables a quick feedback cycle that isn’t possible in most other languages. I strongly recommend that you use it frequently because you’ll be able to quickly check your understanding of Clojure as you learn. Besides that, REPL development is an essential part of the Lisp experience, and you’d really be missing out if you didn’t use it.

To start a REPL, run this:
```
lein repl
```
The output should look like this:
```clj
nREPL server started on port 28925
REPL-y 0.1.10
Clojure 1.9.0
    Exit: Control+D or (exit) or (quit)
Commands: (user/help)
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
          (user/sourcery function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
Examples from clojuredocs.org: [clojuredocs or cdoc]
          (user/clojuredocs name-here)
          (user/clojuredocs "ns-here" "name-here")
clojure-noob.core=>
```
The last line, `clojure-noob.core=>`, tells you that you’re in the `clojure-noob.core` namespace. You’ll learn about namespaces later, but for now notice that the namespace basically matches the name of your `src/clojure_noob/core.clj` file. Also, notice that the REPL shows the version as Clojure 1.9.0, but as mentioned earlier, everything will work okay no matter which version you use.

The prompt also indicates that your code is loaded in the REPL, and you can execute the functions that are defined. Right now only one function, `-main`, is defined. Go ahead and execute it now:
```clj
clojure-noob.core=> (-main)
```
It should print out
```
I'm a little teapot!
nil
```
Well done! You just used the REPL to evaluate a function call. Try a few more basic Clojure functions:
```clj
clojure-noob.core=> (+ 1 2 3 4)
10
clojure-noob.core=> (* 1 2 3 4)
24
clojure-noob.core=> (first [1 2 3 4])
1
```
Awesome! You added some numbers, multiplied some numbers, and took the first element from a vector. You also had your first encounter with weird Lisp syntax! All Lisps, Clojure included, employ prefix notation, meaning that the operator always comes first in an expression. If you’re unsure about what that means, don’t worry. You’ll learn all about Clojure’s syntax soon.

Conceptually, the REPL is similar to Secure Shell (SSH). In the same way that you can use SSH to interact with a remote server, the Clojure REPL allows you to interact with a running Clojure process. This feature can be very powerful because you can even attach a REPL to a live production app and modify your program as it runs. For now, though, you’ll be using the REPL to build your knowledge of Clojure syntax and semantics.

One more note: going forward, this book will present code without REPL prompts, but please do try the code! Here’s an example:
```clj
(do (println "no prompt here!")
   (+ 1 3))
; => no prompt here!
; => 4
```
When you see code snippets like this, lines that begin with `; =>` indicate the output of the code being run. In this case, the text no prompt here should be printed, and the return value of the code is 4.

## 2 Clojure Editors
At this point you should have the basic knowledge you need to begin learning the Clojure language without having to fuss with an editor or integrated development environment (IDE). But if you do want a good tutorial on a powerful editor, Chapter 2 covers Emacs, the most popular editor among Clojurists. You absolutely do not need to use Emacs for Clojure development, but Emacs offers tight integration with the Clojure REPL and is well-suited to writing Lisp code. What’s most important, however, is that you use whatever works for you.

If Emacs isn’t your cup of tea, here are some resources for setting up other text editors and IDEs for Clojure development:
- This YouTube video will show you how to set up Sublime Text 2 for Clojure development: http://www.youtube.com/watch?v=wBl0rYXQdGg/.
- Vim has good tools for Clojure development. This article is a good starting point: http://mybuddymichael.com/writings/writing-clojure-with-vim-in-2013.html.
- Counterclockwise is a highly recommended Eclipse plug-in: https://github.com/laurentpetit/ccw/wiki/GoogleCodeHome.
- Cursive Clojure is the recommended IDE for those who use IntelliJ: https://cursiveclojure.com/
Nightcode is a simple, free IDE written in Clojure: https://github.com/oakes/Nightcode/.

## 3 How to Use Emacs, an Excellent Clojure Editor
### 3.0 Foreword
On your journey to Clojure mastery, your editor will be your closest ally. I highly recommend working with Emacs, but you can, of course, use any editor you want. If you don’t follow the thorough Emacs instructions in this chapter, or if you choose to use a different editor, it’s worthwhile to at least invest some time in setting up your editor to work with a REPL. Two alternatives that I recommend and that are well regarded in the community are Cursive and Nightcode.

The reason I recommend Emacs is that it offers tight integration with a Clojure REPL, which allows you to instantly try out your code as you write. That kind of tight feedback loop will be useful while learning Clojure and, later, when writing real Clojure programs. Emacs is also great for working with any Lisp dialect; in fact, Emacs is written in a Lisp dialect called Emacs Lisp (elisp).

By the end of this chapter, your Emacs setup will look something like a following figure.

![Emacs configuration](https://www.braveclojure.com/assets/images/cftbat/basic-emacs/emacs-final.png)

To get there, you’ll start by installing Emacs and setting up a new-person-friendly Emacs configuration. Then you’ll learn the basics: how to open, edit, and save files, and how to interact with Emacs using essential key bindings. Finally, you’ll learn how to actually edit Clojure code and interact with the REPL.

You should use the latest major version of Emacs, Emacs 24, for the platform you’re working on:
- OS X Install vanilla Emacs as a Mac app from http://emacsformacosx.com. Other options, like Aquamacs, are supposed to make Emacs more “Mac-like,” but they’re problematic in the long run because they’re set up so differently from standard Emacs that it’s difficult to use the Emacs manual or follow along with tutorials.
- Ubuntu Follow the instructions at https://launchpad.net/~cassou/+archive/emacs.
- Windows You can find a binary at http://ftp.gnu.org/gnu/emacs/windows/. After you download and unzip the latest version, you can run the Emacs executable under bin\runemacs.exe.

After you’ve installed Emacs, open it. You should see something like Figure 2-2.

### 3.1 Configuration
I’ve created a repository of all the files you need to configure Emacs for Clojure, available at https://github.com/flyingmachine/emacs-for-clojure/archive/book1.zip.

NOTE: These tools are constantly being updated, so if the instructions below don't work for you or you want to use the latest configuration, please read the instructions at https://github.com/flyingmachine/emacs-for-clojure/.

Do the following to delete your existing Emacs configuration and install the Clojure-friendly one:
1. Close Emacs.
2. Delete `~/.emacs` or `~/.emacs.d` if they exist. (Windows users, your emacs files will probably live in `C:\Users\your_user_name\AppData\Roaming\`. So, for example, you would delete `C:\Users\jason\AppData\Roaming\.emacs.d.`) This is where Emacs looks for configuration files, and deleting these files and directories will ensure that you start with a clean slate.
3. Download the Emacs configuration zip file listed above and unzip it. Its contents should be a folder, `emacs-for-clojure-book1`. Run `mv path/to/emacs-for-clojure-book1 ~/.emacs.d`.
4. Open Emacs.

When you open Emacs, you may see a lot of activity as Emacs downloads a bunch of useful packages. Once the activity stops, go ahead and just quit Emacs, and then open it again. (If you don't see any activity, that's OK! Quit and restart Emacs just for funsies.) After you do so, you should see a window like the one in the next figure.

![Initial emacs screen](https://www.braveclojure.com/assets/images/cftbat/basic-emacs/emacs-configged.png)

### 3.2 Emacs Escape Hatch
Before we dig in to the fun stuff, you need to know an important Emacs key binding: `ctrl-G`. This key binding quits whatever Emacs command you’re trying to run. So if things aren’t going right, hold down ctrl, press G, and then try again. It won’t close Emacs or make you lose any work; it’ll just cancel your current action.

### 3.3 Emacs Buffers

All editing happens in an Emacs buffer. When you first start Emacs, a buffer named `*scratch*` is open. Emacs will always show you the name of the current buffer at the bottom of the window.

By default, the *scratch* buffer handles parentheses and indentation in a way that’s optimal for Lisp development but is inconvenient for writing plain text. Let’s create a fresh buffer so we can play around without having unexpected things happen. To create a buffer, do this:

Hold down ctrl and press X.
Release ctrl.
Press B.
We can express the same sequence in a more compact format: C-x b.

After performing this key sequence, you’ll see a prompt at the bottom of the application.
This area is called the minibuffer, and it is where Emacs prompts you for input. Right now it’s prompting us for a buffer name. You can enter the name of a buffer that is already open, or you can enter a new buffer name. Type in emacs-fun-times and press enter. You should now see a completely blank buffer and can just start typing. You’ll find that keys mostly work the way you’d expect. Characters appear as you type them. The up, down, left, and right arrow keys move you as you’d expect, and enter creates a new line.

You’ll also notice that you’re not suddenly sporting a bushy Unix beard or Birkenstocks (unless you had them to begin with). This should help ease any lingering trepidation you feel about using Emacs. When you’re done messing around, go ahead and kill the buffer by typing C-x k enter. (It might come as a surprise, but Emacs is actually quite violent, making ample use of the term kill.)

Now that you’ve killed the `emacs-fun-times` buffer, you should be back in the `*scratch*` buffer. In general, you can create as many new buffers as you want with C-x b. You can also quickly switch between buffers using the same command. When you create a new buffer this way, it exists only in memory until you save it as a file; buffers aren’t necessarily backed by files, and creating a buffer doesn’t necessarily create a file. Let’s learn about working with files.

### 3.4 Working with Files
The key binding for opening a file in Emacs is `C-x C-f`. Notice that you’ll need to hold down `ctrl` when pressing both `X` and `F`. After you do that, you’ll get another minibuffer prompt. Navigate to `~/.emacs.d/customizations/ui.el`, which customizes the way Emacs looks and how you can interact with it. Emacs opens the file in a new buffer with the same name as the filename. Let’s go to line 37 and uncomment it by removing the leading semicolons. It will look like this:
```clj
(setq initial-frame-alist '((top . 0) (left . 0) (width . 120) (height . 80)))
```
Then change the values for `width` and `height`, which set the dimensions in characters for the active window. By changing these values, you can set the Emacs window to open at a certain size every time it starts. Try something small at first, like 80 and 20:
```clj
(setq initial-frame-alist '((top . 0) (left . 0) (width . 80) (height . 20)))
```
Now save your file with the following key binding: `C-x C-s`. You should get a message at the bottom of Emacs like Wrote /Users/snuffleupagus/.emacs.d/customizations/ui.el. You can also try saving your buffer using the key binding you use in other applications (for example, ctrl-S or cmd-S). The Emacs configuration you downloaded should allow that to work, but if it doesn’t, it’s no big deal.

Go through that same process a couple of times until Emacs starts at a size that you like. Or just comment out those lines again and be done with it (in which case Emacs will open at its default width and height). If you’re done editing ui.el, you can close its buffer with `C-x k`.

Let’s recap:

1. In Emacs, editing takes place in buffers.
2. To switch to a buffer, use C-x b and enter the buffer name in the minibuffer.
3. To create a new buffer, use C-x b and enter a new buffer name.
4. To open a file, use C-x C-f and navigate to the file.
5. To save a buffer to a file, use C-x C-s.
6. To create a new file, use C-x C-f and enter the new file’s path. When you save the buffer, Emacs will create the file on the filesystem.

### 3.5 Emacs Is a Lisp Interpreter
The term key binding derives from the fact that Emacs binds keystrokes to commands, which are just elisp functions (I’ll use command and function interchangeably). For example, `C-x b` is bound to the function `switch-to-buffer`. Likewise, `C-x C-s` is bound to `save-file`.

But Emacs goes even further than that. Even simple keystrokes like f and a are bound to a function, in this case `self-insert-command`, the command for adding characters to the buffer you’re editing.

From Emacs’s point of view, all functions are created equal, and you can redefine all functions, even core functions like `save-file`. You probably won’t want to redefine core functions, but you can.

You can also run commands by name, without a specific key binding, using M-x function-name (for example, `M-x` `save-buffer`). `M` stands for meta, a key that modern keyboards don’t possess but which is mapped to alt on Windows and Linux and option on Macs. `M-x` runs the `smex` command, which prompts you for the name of another command to be run.

Now that you understand key bindings and functions, you’ll be able to understand what modes are and how they work.

### 3.6 Modes
An Emacs mode is a collection of key bindings and functions that are packaged together to help you be productive when editing different types of files. (Modes also do things like tell Emacs how to do syntax highlighting, but that’s of secondary importance, and I won’t cover that here.)

For example, when you’re editing a Clojure file, you’ll want to load Clojure mode. Right now I’m writing a Markdown file and using Markdown mode, which has lots of useful key bindings specific to working with Markdown. When editing Clojure, it’s best to have a set of Clojure-specific key bindings, like `C-c C-k` to load the current buffer into a REPL and compile it.

Modes come in two flavors: __major modes__ and __minor modes__. Markdown mode and Clojure mode are major modes. Major modes are usually set by Emacs when you open a file, but you can also set the mode explicitly by running the relevant Emacs command, for example with `M-x clojure-mode` or `M-x major-mode`. Only one major mode is active at a time.

You can see which modes are active on the mode line, as shown in the next figure.
![Major mode indicator](https://www.braveclojure.com/assets/images/cftbat/basic-emacs/emacs-mode-line.png)

If you open a file and Emacs doesn’t load a major mode for it, chances are that one exists. You’ll just need to download its package. Speaking of which . . .

### 3.7 Installing Packages
Many modes are distributed as packages, which are just bundles of elisp files stored in a package repository. Emacs 24, which you installed at the beginning of this chapter, makes it very easy to browse and install packages. `M-x` `package-list-packages` will show you almost every package available; just make sure you run `M-x` `package-refresh-contents` first so you get the latest list. You can install packages with `M-x` `package-install`.

You can also customize Emacs by loading your own elisp files or files you find on the Internet. The “Beginner’s Guide to Emacs” (found at http://www.masteringemacs.org/articles/2010/10/04/beginners-guide-to-emacs/) has a good description of how to load customizations under the section “Loading New Packages” toward the bottom of the article.

### 3.8 Core Editing Terminology and Key Bindings
If all you want to do is use Emacs like a text editor, you can skip this section entirely! But you’ll be missing out on some great stuff. In this section, we’ll go over key Emacs terms; how to select, cut, copy, and paste text; how to select, cut, copy, and paste text (see what I did there? Ha ha ha!); and how to move through the buffer efficiently.

To get started, open a new buffer in Emacs and name it jack-handy. Then enter the following Jack Handy quotations:
```
If you were a pirate, you know what would be the one thing that would
really make you mad? Treasure chests with no handles. How the hell are
you supposed to carry it?!

The face of a child can say it all, especially the mouth part of the
face.

To me, boxing is like a ballet, except there's no music, no
choreography, and the dancers hit each other.
```
Use this example to experiment with navigation and editing in this section.

#### 3.8.0 Point
If you’ve been following along, you should see a red-orange rectangle in your Emacs buffer. This is the cursor, and it’s the graphical representation of the point. Point is where all the magic happens: you insert text at point, and most editing commands happen in relation to point. And even though your cursor appears to rest on top of a character, point is actually located between that character and the previous one.

For example, place your cursor over the f in If you were a pirate. Point is located between I and f. Now, if you use C-k, all the text from the letter f onward will disappear. `C-k` runs the command `kill-line`, which kills all text after point on the current line (I’ll talk more about killing later). Undo that change with `C-/`. Also, try your normal OS key binding for undo; that should work as well.

#### 3.8.1 Movement
You can use your arrow keys to move point just like any other editor, but many key bindings allow you to navigate more efficiently, as shown in Table 2-1.

Table 2-1: Key Bindings for Navigating Text
Keys	Description
`C-a`	Move to beginning of line.
`M-m`	Move to first non-whitespace character on the line.
`C-e`	Move to end of line.
`C-f`	Move forward one character.
`C-b`	Move backward one character.
`M-f`	Move forward one word (I use this a lot).
`M-b`	Move backward one word (I use this a lot, too).
`C-s`	Regex search for text in current buffer and move to it. Press `C-s` again to move to next match.
`C-r`	Same as `C-s`, but search in reverse.
`M-<`	Move to beginning of buffer.
`M->`	Move to end of buffer.
`M-g g`	Go to line.
Go ahead and try out these key bindings in your jack-handy buffer!

#### 3.8.2 Selection with Regions
In Emacs, we don’t select text. We create regions, and we do so by setting the mark with `C-spc` (ctrl-spacebar). Then, when you move point, everything between mark and point is the region. It’s very similar to shift-selecting text for basic purposes.

For example, do the following in your jack-handy buffer:
1. Go to the beginning of the file.
2. Use `C-spc`.
3. Use `M-f` twice. You should see a highlighted region encompassing If you.
4. Press backspace. That should delete If you.

One cool thing about using mark instead of Shift-selecting text is that you’re free to use all of Emacs’s movement commands after you set the mark. For example, you could set a mark and then use `C-s` to search for some bit of text hundreds of lines down in your buffer. Doing so would create a very large region, and you wouldn’t have to strain your pinky holding down shift.

Regions also let you confine an operation to a limited area of the buffer. Try this:

Create a region encompassing The face of a child can say it all.
Use `M-x` `replace-string` and replace face with head.
This will perform the replacement within the current region rather than the entire buffer after point, which is the default behavior.

#### 3.8.3 Killing and the Kill Ring
In most applications we can cut text, which is only mildly violent. We can also copy and paste. Cutting and copying add the selection to the clipboard, and pasting copies the contents of the clipboard to the current application. In Emacs, we take the homicidal approach and kill regions, adding them to the kill ring. Don’t you feel braver and truer knowing that you’re laying waste to untold kilobytes of text? We can then yank, inserting the most recently killed text at point. We can also copy text to the kill ring without actually killing it.

Why bother with all this morbid terminology? Well, first, so you won’t be frightened when you hear someone talking about killing things in Emacs. But more important, Emacs allows you to do tasks that you can’t do with the typical cut/copy/paste clipboard feature set.

Emacs stores multiple blocks of text on the kill ring, and you can cycle through them. This is cool because you can cycle through to retrieve text you killed a long time ago. Let’s see this in action: