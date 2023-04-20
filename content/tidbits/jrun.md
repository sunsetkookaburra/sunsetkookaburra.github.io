+++
title = "jrun.sh"

[[extra.style]]
rule = """
  pre {
    border-radius: 1rem;
    overflow-x: scroll;
  }
  pre > code {
    display: block;
    padding: 2rem;
    float: left;
  }
"""
+++

A simple Java project build script.

* Source code (`.java`) in `src/`
* Assets and resources (non-`.java`) co-located in `src/`
* Internal structure of output `.jar` file mimcs `src/`, but with compiled `.class` files replacing `.java` source files.

```sh
#!/bin/sh
# jrun.sh

if [ "$#" != "1" ] || [ "$1" = "-h" ] || [ "$1" = "--help" ]
then
  echo "jrun <MainClass>"
  return
fi

if ! [ -d "src" ]
then
  echo "No src/ directory found."
  return 1
fi

list_sources () { find -type f -name '*.java' -printf '%P\0'; }
list_resources () { find -type f -not -name '*.java' -printf '%P\0'; }

mkdir -p build/ &&

# Change into the src/ directory
# List and compile all .java files, and output into the ../build/ directory.
echo "Compiling .java files ..." &&
pushd src/ && list_sources | xargs -0 javac -d ../build/ && popd &&

# Create a .jar file to bundle & distribute your program.
#   c = create new archive (jarfile)
#   v = verbose
#   f = archive/jar filename
#   e = entrypoint (main class) name
#   -C build/ . = Change into build dir and include class files from there
#     (so they don't end up in a build folder in the jar)
echo "Making .jar from .class files ..." &&
jar cvfe "$1".jar "$1" -C build/ . &&

# Change into src/ dir, and add resources to jar file
#   u = update jar file, add new items
#   $resources = (unquoted important) list the resources files (.jpeg, .xml, etc) to include
echo "Adding resources to .jar ..." &&
pushd src/ && list_resources | xargs -0 jar uvf ../"$1".jar && popd &&

# Run the binary
echo "Done." &&
java -jar "$1".jar &&

# Pause after finish (useful in '$ clear; jrun MyApp' one-liners)
read -n 1 -p "Press any key to continue ..."
```
