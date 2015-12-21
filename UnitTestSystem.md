The summary and mysql-summary tools are written in Bash, and there is a set of unit tests for them.  Here's an overview of how the testing system works.

# Running the Tests #

To run the tests, change to the t/ directory and execute the test script:

`./summary-tests`
or
`./mysql-summary-tests`

The result should be one line of output per test.  The line begins with OK if the test passes, and NOK if it fails.

# How Tests Work #

The test script works by sourcing the appropriate script, and then for each file found in the subdirectory named auto-tests-$scriptname, it executes that file.  The file is itself a script, which accepts two arguments: the name of the file where the correct output should be found, and the name of the file where input will be sought.  These individual test scripts just set up the input and desired output for the test itself. Then the test script simply executes the function named in a comment on the second line of the individual test, redirects the output to a temporary location, and compares it with the desired output.

# How to Write Tests #

Tests rely on the appropriate scripts being source-able and on each bit of functionality being in a little function of its own.  The script is source-able if it doesn't execute anything unless it's being executed directly from the command-line.  There'll be a main() function which is executed from the very bottom of the script, depending on $0.

To write tests, you really just need to write your new functionality into a little function that takes its input from something like /tmp/aspersa and prints output to STDOUT, and then copy-paste an existing test.  Change the second line (after the shebang) to mention the name of your new function.  Change the second paragraph to generate your desired output somehow.  Change the third paragraph to generate your desired input.  Run the test-harness script and see if you get an OK.