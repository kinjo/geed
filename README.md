Geed
====

When you want to insert the comments to any functions defined in a.c, b.c, d.c and z.c, 
you try to open and edit the files many times. But, Geed will do that at a time.

Geed generates the ed commands to insert, replace and delete any lines scattered in several files.
After that Ed will do them.

Geed is a collection of Perl scripts. Currently, there are two scripts available geed-repl and geed-del.

NOTE: If you use Geed without a version control system, your source code may be extensively 
destroyed. So it is strongly recommend that you operate with a version control systems such as Git.

# Install

Just put the scripts in your ~/bin directory and go.

# How to use

## geed-repl

geed-repl generates the ed commands from grep output
to replace and insert any lines to a number of files at a time.

For example, there are 2 files a.c and b.c.

a.c

    int
    main()
    {
      foo();
      return 0;
    }

b.c

    void foo()
    {
    }

Now, if you want to insert some comments before the two functions main() and foo(), 
following steps will be useful for you.

At first, extract the target lines to a file using grep and then edit it.
NOTE: filename and line number are required to geed-repl.

    grep -Hn '^[a-z]' *.c > grep.out

Before editing, grep.out is
 
    a.c:1:int
    a.c:2:main()
    b.c:1:void foo()

Edit grep.out as below.

    a.c:1:/* main */
    int
    a.c:2:main()
    b.c:1:/*
     * foo
     */
    void foo()

Run geed-repl with ed.

    geed-repl < grep.out | ed

In now,

a.c

    /* main */
    int
    main()
    {
      foo();
      return 1;
    }

b.c
 
    /*
     * foo
     */
    void foo()
    {
    }

## geed-del

geed-del generates the ed commands to delete any lines.
You can extract deleting lines using grep and remove lines if you don't want to delete them.

For example, if you want to remove all comments inserted at the previous section, follow below steps.

Extract comments using grep.

    grep -Hn -e '/\*' -e '\ *\*' *.c > grep.out

In this case, since there is no line you don't want to delete, skip to edit grep.out,
straightly execute this.

    geed-del < grep.out | ed

In now,

a.c

    int
    main()
    {
      foo();
      return 1;
    }

b.c

    void foo()
    {
    }

# License

Copyright (c) 2013 Takumi KINJO

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
