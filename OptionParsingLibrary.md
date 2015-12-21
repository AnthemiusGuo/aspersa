Aspersa includes a command-line option parsing libary (see [issue 67](https://code.google.com/p/aspersa/issues/detail?id=67)) that automatically generates command-line option handling code in Bash from the program's usage/help text.  This avoids writing all that code manually, and makes sure that the help text and the program's behavior are really in sync.

The code is stored in the file lib/options.  It has the following characteristics.

# Help/Usage Text #

The help and usage text should be in the following format:

```
# Print a usage message and exit.
usage() {
   if [ "${OPT_ERR}" ]; then
      echo "${OPT_ERR}"
   fi
   cat <<-USAGE
Usage: $0 [OPTIONS] FILE [FILE...]
   $0 does wizzle-bang.
Options: (required: -a)
   -a              Foobity foo.
                   Second line, different from the first.
   -b BARBAR       Sets the bar.  Specify one of the following:
      bingabang)   # Bing the bang
      wobble)      # Different from wibble
   -c BING         Apple pie (default 5).
	USAGE
   exit 1
}
```

The text that matters is between the two lines that say USAGE, which must be formatted just right.  Each line starting with a single dash followed by a single letter is an option.  If it is then followed by a WORD, the option takes an argument.  If it also says (default TEXT), the default value is TEXT.

Options that have the text "one of the following:" will begin a CASE statement.  Lines that follow and have a case-statement-syntax word on the front, as in option -b above, define acceptable values for that option's argument.

If the line beginning Options: continues with (required: -OPTIONS), as above, then each OPTION is required.

If there is an error during the option processing, then the usage function is called, and the OPT\_ERR variable is set to a message explaining the problem.

# Using the Code #

To use the code, either run lib/options's function generate\_parse\_options() against your program and save and embed the resulting text at the point where you want the options to be processed, or embed that function's source into your script, and source the results of the function.  The latter is preferred, and can be done easily as follows:

```
   . <(generate_parse_options)
```

After doing that, an environment variable OPT

<option\_name>

 is set for each option.  For example -b sets OPT\_b.