Download Link: https://assignmentchef.com/product/solved-cmsc216-project-3-user-interface
<br>
5/5 - (1 vote)

1 Overview1.1 user interface program

For this project you will write a text-based user interface to the document manager system you implemented in project #2. In addition, you will add some extra functionality to your system. There are two deadlines associated with the project. Those deadlines are:

2 Objectives

To practice text parsing and file I/O.

2.1 Obtain the project files

To obtain the project files copy the folder project3 available in the 216 public directory to your 216 direc- tory. Keep in mind that the Makefile and document.h files for this project are different from the ones used in project2.

2.2 Fixing problems with your project #2 code

After the late deadline for project2 you will be able to see results for release/secret tests in the submit server. A TA during office hours (and only during office hours) will be able to show you any test and why the test failed (if that is the case). You are responsible for fixing your code before submitting this project. Keep in mind that if you passed all the project2 tests that does not mean you don’t have bugs. In this project we will be testing your document functions again so it is in your best interest to test your code thoroughly.

3 Specification3.1 Document manager update

You need to add two functions to your document manager system. Remember to use the provided document.h file (not the one from project #2).

<ol>

 <li>int load_file(Document *doc, const char *filename) – This function is similar to load document, except data will will be loaded from a file instead of using an array. By default a paragraph will be added and any blank lines (line with only spaces as defined by isspace()) will mark the beginning of a new paragraph. The function will fail and return FAILURE if doc is NULL, filename is NULL, or if opening the file failed; otherwise the function will return SUCCESS. Notice no error message will be generated if the file cannot be opened.</li>

 <li>int save_document(Document *doc, const char *filename)-Thisfunctionwillprinttheparagraphs associated with a document to the specified file (overwriting the file). Each paragraph will be separated</li>

</ol>

1

by a newline. The function will fail and return FAILURE if doc is NULL, filename is NULL, or the file cannot be opened; otherwise the function will return SUCCESS. Notice no error message will be generated if the file cannot be opened.

3.2 Method of operation

Your program will be in a file named user_interface.c. A user calls your program in one of two ways (assuming the executable is named user interface):

<pre>   user_interface   user_interface filename</pre>

The program should have zero or one arguments (in addition to the executable name) on the command line; if there are more the program prints the following usage message to standard error, and exits with exit code EX_USAGE 1.

<pre>Usage: user_interfaceUsage: user_interface &lt;filename&gt;</pre>

If there is no file specified when the program is started, the program should read its data from standard input. The program will display a prompt (represented by &gt;) after which commands will be entered. If a file is named, however, the program reads its data from that file; in this case no prompt will be used.

In case of an error opening the file your program should print (to standard error) the message ”FILENAME cannot be opened.” where FILENAME represents the file name. The program will then exit with the exit code EX_OSERR.

Upon starting execution your program should initialize a single document with the name ”main document”, and perform operations on that document as instructed by the commands the program reads.

Make sure you name the file with your program user interface.c. This program will include document.h (the version provided for this project and not the one from project #2).

3.3 File format

3.3.1 Valid Lines

An input file (or input coming from standard input) contains multiple lines with commands, and the com- mands are executed in the order they are encountered. No valid line can be more than 1024 characters (includ- ing the newline character). A valid line takes one of three forms:

1. a comment, where the first non-whitespace character is a hash symbol (‘#’)2. a command, where the line is composed of one or more strings of non-whitespace characters3. a blank line, where the line contains 1 or more spaces (as defined by the isspace() function in ctype.h)

For example, the following file contains valid lines:

<pre># creating a paragraph and inserting some linesadd_paragraph_after 0add_line_after 1 0 *first line of the document</pre>

<pre>add_line_after 1 1 *second line of the document   # let’s print it</pre>

<pre>print_documentquit</pre>

1ThisandtheotherexitcodesbeginningwithEX mentionedhereareallobtainedbyincluding&lt;sysexits.h&gt;inyourCprogram file.

2

Valid commands must follow one of the formats specified in Section 3.4 below.

3.3.2 Invalid Lines/Commands

If your program encounters an invalid line it should print the message ”Invalid Command”. An invalid line includes not only an invalid command, but a command without the expected values. For example, the add paragraph after command requires an integer. If the value provided is not an integer the command will be considered invalid and the message ”Invalid Command” will be generated. Notice the program will not end when an invalid command is provided. Make sure you print to the standard output and not to the standard error.

3.4 Commands

Unless output is associated with a command the successful execution of a command will not generate any con- firmation message (similar to successful execution of commands in Unix). If a command cannot be succesfully executed the message ”COMMAND NAME failed”, where COMMAND NAME represents the command, should be printed. Make sure you print to the standard output and not to the standard error.

Any number of spaces can appear between the different elements of a command, and before and after a command. A blank line (as defined above) and a comment will be ignored (no processing). When a comment or blank line is provided, and standard input is being used, a new prompt will be generated.

Although the quit and exit commands will end/terminate the command processor, the command processor should terminate when end of file is seen. A set of commands in a file do not need quit nor exit.

1. add_paragraph_after PARAGRAPH_NUMBERThis command will add a paragraph to the document. The ”Invalid Command” message will be gener-

ated when:

<ol>

 <li>PARAGRAPH NUMBER does not represent a number</li>

 <li>PARAGRAPH NUMBER is a negative value</li>

 <li>PARAGRAPH NUMBER is missing</li>

 <li>Additional information is provided after the PARAGRAPH NUMBER</li>

</ol>

If the command cannot be successfully executed the message ”add paragraph after failed” will be gen- erated.

2. add_line_after PARAGRAPH_NUMBER LINE_NUMBER * LINEThis command will add a line after the line with the specified line number. The line to add will appear

after the * character. The ”Invalid Command” message will be generated when:

<ol>

 <li>PARAGRAPH NUMBER does not represent a number</li>

 <li>PARAGRAPH NUMBER is a negative value or 0</li>

 <li>PARAGRAPH NUMBER is missing</li>

 <li>LINE NUMBER does not represent a number</li>

 <li>LINE NUMBER is a negative value</li>

 <li>LINE NUMBER is missing</li>

 <li>* is missing</li>

</ol>

If the command cannot be successfully executed the message ”add line after failed” will be generated. 3. print_document

This command will print the document information (print document function output). The ”Invalid Command” message will be generated if any data appears after print document.

3

<ol start="4">

 <li>quitThis command will exit the user interface. The ”Invalid Command” message will be generated whenany data appears after quit.</li>

 <li>exitThis command will exit the user interface. The ”Invalid Command” message will be generated when any data appears after exit.</li>

 <li>append_line PARAGRAPH_NUMBER * LINEThis command will append a line to the specified paragraph. The line to add will appear after the *character. The ”Invalid Command” message will be generated when:

  <ol>

   <li>PARAGRAPH NUMBER does not represent a number</li>

   <li>PARAGRAPH NUMBER is a negative value or 0</li>

   <li>PARAGRAPH NUMBER is missing</li>

   <li>* is missing</li>

  </ol>If the command cannot be successfully executed the message ”append line failed” will be generated.</li>

 <li>remove_line PARAGRAPH_NUMBER LINE_NUMBERThis command will remove the specified line from the paragraph. The ”Invalid Command” message will be generated when:

  <ol>

   <li>PARAGRAPH NUMBER does not represent a number</li>

   <li>PARAGRAPH NUMBER is a negative value or 0</li>

   <li>PARAGRAPH NUMBER is missing</li>

   <li>LINE NUMBER does not represent a number</li>

   <li>LINE NUMBER is a negative value or 0</li>

   <li>LINE NUMBER is missing</li>

   <li>Any data appears after the line number</li>

  </ol>If the command cannot be successfully executed the message ”remove line failed” will be generated.</li>

 <li>load_file FILENAMEThis command will load the specified file into the current document. The ”Invalid Command” message will be generated when:

  <ol>

   <li>FILENAME is missing</li>

   <li>Any data appears after FILENAME</li>

  </ol>If the command cannot be successfully executed the message ”load file failed” will be generated.</li>

 <li>replace_text “TARGET” “REPLACEMENT”This command will replace the string ”TARGET” with ”REPLACEMENT”. The ”Invalid Command” message will be generated when:

  <ol>

   <li>Both ”TARGET” and ”REPLACEMENT” are missing</li>

   <li>Only ”TARGET” is provided</li>

  </ol>For this command you can assume that if ”TARGET” and ”REPLACEMENT” are present there is no additional data after ”REPLACEMENT”.If the command cannot be successfully executed the message ”replace text failed” will be generated. 4</li>

</ol>

<ol start="10">

 <li>highlight_text “TARGET”This command will highlight the string ”TARGET”. The ”Invalid Command” message will be generatedwhen ”TARGET” is missing.For this command you can assume that if ”TARGET” is present there is no additional data after it. Notice no fail message is associated with this command; either the text was highlighted or not.</li>

 <li>remove_text “TARGET”This command will remove the string ”TARGET”. The ”Invalid Command” message will be generatedwhen ”TARGET” is missing.For this command you can assume that if ”TARGET” is present there is no additional data after it. Notice no fail message is associated with this command; either a deletion took place or not.</li>

 <li>save_document FILENAMEThis command will save the curent document to the specified file. The ”Invalid Command” messagewill be generated when:

  <ol>

   <li>FILENAME is missing.</li>

   <li>Any data appears after the filename.</li>

  </ol>If the command cannot be successfully executed the message ”save document failed” will be generated.</li>

 <li>reset_documentThis command will reset the curent document. The ”Invalid Command” message will be generated when any data appears after reset document. Notice no fail message will be associated with reset document.</li>

</ol>

3.5 Important Points and Hints

4

<ol>

 <li>Data should only be allocated statically. You may not use malloc() etc.</li>

 <li>Do not use perror to generate error messages; use fprintf and stderr instead.</li>

 <li>IMPORTANT: You may not use memset, strtok, strtok r, memcpy in this project.</li>

 <li>IMPORTANT. You may not use regular expressions for this project; if you do you will lose at least 30 points. Regular expressions can be used in the format string of a scanf statement in order to recognize string patterns. The following are characters typically associated with regular expressions:<pre>         [,],*,^,-,$,?</pre></li>

 <li>Do not include .c files using #include.</li>

 <li>You can assume a filename will not exceed 80 characters.</li>

 <li>If you remove a line that line should not be printed (no blank line for it).</li>

 <li>If you remove all the lines from a paragraph, the paragraph will not be removed (paragraph count will stay the same).</li>