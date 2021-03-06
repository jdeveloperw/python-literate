Literate helps you author reproducible HTML reports from Python. Integrate Python code and Markdown documentation in `.pylit` files (or `.py.md` if you prefer.) Literate will capture the output of your code and weave it in with your thoughts and the code itself.

    Here's what a `.pylit` document can look like:

        numbers = [1, 2, 3]
        print [float(number * 3) for number in numbers]
        first, middle, last = numbers

    And then we can write more, and talk about our calculations 
    like {first} and {last}.

Indented text is Python. Non-indented text is Markdown. You can use any 
variables from Python in Markdown, and they will be interpolated like 
you're used to from regular Python string formatting.

We can run that through Literate with the command:

    ./literate untangle myreport.pylit --capture --print

Or save it to somewhere:

    ./literate untangle ./myreport.pylit ./build --capture

The output will look like: 

> Here's what a `.pylit` document can look like:
> 
> > numbers = [1, 2, 3]
> > print [float(number * 3) for number in numbers]
> > first, middle, last = numbers
> > `[3.0, 6.0, 9.0]`
> 
> And then we can write more, and talk about our calculations 
> like 1 and 3.

If you want to capture output (like in the example above), pass the `--capture` flag
to the command line interface. It's great for research notebooks, and akin to 
IPython notebooks, R Markdown, Sweave and Pweave.

For regular literate programming (just code and your explanations of that code)
simply leave off the `--capture` flag: it's what Literate does by default.

## The command-line interface

To find out more about how to use the command-line interface, try: 

    ./literate untangle --help
    ./literate run --help

## Working with Literate Python files

The Python interpreter doesn't understand Literate Python by default, but that's easily fixed: 

    # add support for importing Literate Python
    # and then import our `myreport.pylit` file
    # like a regular Python source file
    import literate
    import myreport

Just like `.py` files generate `.pyc` bytecode, `.pylit` files sometimes generate 
a `.py` representation. Running `literate untangle myreport.pylit` or doing 
`import myreport` produces a `myreport.py` file.

**Never put source code in, or edit, a `.py` file if there's a `.pylit` file 
with the same basename. It may be overwritten and you will lose whatever code 
you had in your `.py` file.**

## Output formats

Literate can output to `.py`, `.md` and `.html`. HTML output is very useful for simple reports.

    ./literate untangle examples/basic examples/basic/build \
        --template examples/basic/template.html \
        --formats html \
        --capture

There's a very basic HTML template built into Literate, but you can also use your own.
Use the `--template` flag, e.g. `--template mytemplate.html`. Your template file should
contain a `<article/>` tag, which is where Literate will put your generated report.

For finer control over layout and report rendering, use the Markdown output (`--formats md`) which 
will contain code, documentation and captured output. With those Markdown files, build your own 
reports using the site generator of your choice. Jekyll's a good one.

## Does it do...

Literate Python is inspired on Literate CoffeeScript rather than the traditional Sweave syntax.
You won't get fine-grained control over which individual chunk to run or display because I 
don't believe that's how literate reports should work.

Don't include code in a `.pylit` file you don't want in the generated output but instead 
put it in library files, it's as simple as that.

## Roadmap

### 1.0rc

* fix './' (which should be the CWD, not the dir of the literate script)
* tests
* Also, when using the Literate style, add Markdown to the Python output
  in docstrings.
* Python output hooks
  - initial support for `matplotlib` plots
* different weaving and untangling modes
  weave
  - literate (code + interpolated documentation)
  - --prose (interpolated documentation)
  - --notebook (code + output + interpolated documentation)
  untangle (--formats)
  - md
  - py
* Make it possible to pass on woven files to a templating language executable
  Ideally: `literate weave . --template jade --object {context} --output build`
  (with {context} and {contextFile} variables)
* A good default template
  - dynamic one-column / two-column
  - works on directories (includes a TOC)
* Review code and put it up on PyPI

### Other

* A Sublime Text syntax highlighter

### Future

* Support for plain text (may make it easier for people who 
  want to use formats other than those we support to DIY)
* Support for D3 plots, tables and other visualizations
* Support for `.md.py` (Python with Markdown docstrings) 
  (I suppose the catch is that the one works with Python out of the box, 
  the other works with Markdown out of the box...)