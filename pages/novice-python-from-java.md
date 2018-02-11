The Novice's Guide to Python 3 from Java

The source for this guide lives in GitHub [here](https://github.com/philvarner/novices-guides/tree/master/novice-python-from-java).

============================

- Python 2 or 3?
- Installing
- Installing Library Modules
- Running
- Variables and Types
- Print
- Importing modules
- Argument Parsing
- Command Line Input
- Reading and Writing Files
- Time
- Built-in Data Structures
- Lists
- Dictionaries
- Tuples
- Strings
- Regexes
- Templates
- Requests
- XML
- JSON
- Running external commands
- Function Definition
- Sorting
- Comprehensions

Overview
--------


 
Python 2 or 3?
--------------
 
Short: Python 3 if you're starting a new project.
 
Long: [Picking an Interpreter — The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest/starting/which-python/)

Installing
--------------
 
Python comes with OS X and Linux, but sometimes it's a bit out of date. See the excellent The Hitchhiker’s Guide to Python! http://docs.python-guide.org/en/latest/ for updated install instructions for Mac or Linux.
 
JetBrains PyCharm http://www.jetbrains.com/pycharm/ is an excellent IDE, and the Community Edition is free. The PEP8 code inspector is great for making you write code in a more Pythonic style.
 
Installing Library Modules
--------------

Python has a huge set of libraries.  The easiest way to install them in with Pip.  "pip" installs libs for Python 2 and "pip3" installs libs for Python 3.

> pip3 install requests
 
For getting started and casual Python use this is fine, but virtualenv is a better way if you need to manage multiple projects with different version dependencies.  See  Virtual Environments — The Hitchhiker's Guide to Python for more info on that. http://docs.python-guide.org/en/latest/dev/virtualenvs/
 
Running
--------------
 
To get a repl:

> python3
 
To exit that repl, use the built-in function quit() or Ctrl-D (i.e. EOF).
 
To run a script, make it executable and then do

> python3 scriptname.py
 
or add #!/usr/bin/env python3 as the first line in the file, and run like

> ./scriptname.py
 
Variables and Types
--------------
 
Dynamically-typed. Variables have no declaration syntax. Variables are not scoped to the immediate enclosing context, so you can assign a new variable inside of an if and it will be visible outside of it. Strings can be surrounded by  ', ", ''', or """ (latter are multi-line). Non-strings must be explicitly converted to strings with the built-in "str" function. Code blocks are notated with indentation of 4 spaces. Boolean values are True and False, and null is None.

Python:

    one = None
    one = 1
    foo = one
    foo = 'bar'  # or "bar"
    is_it = True
    if is_it :
        print("it's true! " + foo + " " + str(one))

> it's true! bar 1

Java:

    Object one = null;
    int one2 = 1;
    int foo = one2;
    String foo2 = "bar";
    boolean is_it = true;
    if (is_it) {
        System.out.println("it's true! " + foo + " " + foo2 + " " + one);
    }
        
Idiomatic Python is to use underscores for variable names instead of camel case. Also, parens are only used for functions and expression grouping, and typically not used in for-in loops and "simple" if/elif/else branches.  Code blocks are defined using indentation of 4 spaces instead of braces.
 
Print
-----

The builtin function print prints things. Note that print is a statement (instead of a function) in Python 2, so you may see code like print foo, which in Python 3 must be done as print(foo).
 
By default, it acts like println and includes the EOL.

Python:

    print('this is printed')
    
Java:

    System.out.println("this is printed');
 
print() also has an option to specify the end of the line, like:

Python:

    print('this print doesn't have an EOL...', end='')  #no EOL
    print('but this one does.')
    
> this print doesn't have an EOL...but this one does.
    
Java:

    System.out.print("this print doesn't have an EOL...")
    System.out.println("but this one does.")

Using '\r' as the EOL character will only bring the cursor back to the front of the line but not advance the line, so you can do "in place" output updates, for example, printing the status of an ongoing operation:

Python:
    
    print('this line will be overwritten by the next one', end='\r')  # carriage return, so the cursor backs up
    print('I overwrote what was here before')
 
> I overwrote what was here before

Java:

    System.out.print("this line will be overwritten by the next one\r");
    System.out.print("I overwrote what was here before");
 
Python allows named parameters instead of only positional ones like Java, so end='' is passing a value for the function parameter named end.
 
When you're printing without an EOL to the console, you often also need to call flush so the text will actually appear, with:

Python:

    import sys
    print('non-flushed output')
    sys.stdout.flush()
 
Java:

    System.out.print("non-flushed output");
    System.out.flush();
 
Because non-strings are not automatically cast to strings like they are in Java, you have to explicitly cast them using the built-in str function:

Python:

    print(str(1))
    print(str(1.0))

Java:

    System.out.println(1);
    System.out.println(1.0);
 

Importing modules
-----------------

The preferred way of importing a module namespace is to put import statements at the top of the file.

Python:

    import time
    print(time.strftime("%a, %d %b %Y %H:%M:%S"))

This form of import simply makes the name of the package visible in the current context.  There is no equivalent to this form in Java, since all classes in the classpath can always be used in their fully-qualified form (e.g., org.joda.time.DateTime).  This is the recommended form, as it makes the origin of functions and classes explicit.  

Java-style imports can be done, but they are generally not used in Pythonic code because it's often difficult to see right away which function is being called.  This is usually only done from the most commonly used functions.

Python:

    from time import *
    print(strftime("%a, %d %b %Y %H:%M:%S"))

Java:

    import static org.joda.time.DateTime.*;
    parse("1997-08-29T17:12:05Z");

If unqualified functions are going to be imported, they should be explicitly named in the import, so that a search of the file reveals which package they're from:

Python:

    from time import strftime
    print(strftime("%a, %d %b %Y %H:%M:%S"))

Java:

    import static org.joda.time.DateTime.parse;
    parse("1997-08-29T17:12:05Z");

Python also support aliasing packages in the import, which can be useful for long or odd package names.  Java does not have this capability, though the related Groovy language does.

Python:

    import psycopg2 as db
    db.connect()

Argument Parsing
----------------

Basic argument parsing can be done with the argv array.  

Python:

    import sys
    print(sys.argv[0])  # the name of the script
    print(sys.argv[1])  # the first argument to the script

Java:

    public static void main(String[] args){
        System.out.println(args[0]);
        System.out.println(args[1]);
    }

The *argparse* module is fantastic for parsing command-line args.  It allows you to do short and long param names, default values, force to specific numeric types, and varargs for things like a list of file names.

Python:
 
    import argparse
     
    parser = argparse.ArgumentParser()
    parser.add_argument('-n', '--name', default='', help='the name to prepend to the report')
    parser.add_argument('-r', '--ratio-threshold', default=0.75, type=float, help='the ratio threshold for two entries are the same')
    parser.add_argument('-d', '--diff-length-threshold', type=int, default=512, help='diff length threshold')
    parser.add_argument('FILE', nargs='+', help='files to parse')
    args = parser.parse_args()
 
    report_name = args.name
    ratio_threshold = args.ratio_threshold
    diff_length_threshold = args.diff_length_threshold
    log_files = args.FILE

 
Command Line Input
------------------

To get input from the console:

Python:

    what = input('what? ')
    print(what)

Java:

    String what = System.console().readLine("what? ");
    System.out.println(what);

 
Reading and Writing Files
-------------------------

Python has resource handlers as part of the language, which allow automatic resource management.  This makes it far cleaner to ensure resource cleanup than using try/finally as Java requires.
 
*read()* on a filehandle reads the entire file into a string.

Python:

    with open('myfile.txt', 'r') as fin:  #open file in read mode
        contents = fin.read()
    print(contents)

Java:

    BufferedReader br = new BufferedReader(new FileReader("myfile.txt"));
    StringBuilder sb = new StringBuilder();
    try {
        String line = br.readLine();
        while (line != null) {
            sb.append(line).append("\n");
            line = br.readLine();
        }
    } finally {
        br.close();
    }
    System.out.println(sb.toString());

There are quite a few libraries which make this easier in Java, such as the Apach Commons IO IOUtils class, but the built-in JDK primitives for IO are complex to use and (most importantly) lead to vastly different ways of doing exactly the same thing, which reduces readability and code comprehension.

Reading one file and writing it to another file:

Python:
 
    with open('myfile.txt', 'r') as fin:  #open file in read mode
        with open('myfile_copy.txt, 'w') as fout:  #open file in write mode
            fout.write(fin.read())
 
*readlines()* is alternative that reads each line into a list, which can be useful for line-oriented processing like log file analysis:

Python:

    with open('myfile.txt', 'r') as fn:
        lines = fn.readlines()
 
    for line in lines:
        print(line)
 
To see if a file exists:

    import os
 
    if os.path.exists('myfile.txt'):
        print(filename + " exists.")
    else :
        print(filename + " doesn't exists.")
 
To list all of the files and directories in a directory:

    for name in os.listdir('.'):
        print(name)

More information about the *os* package can be found at os — [Miscellaneous operating system interfaces](http://docs.python.org/3/library/os.html)
 

Built-in Data Structures
------------------------

Literal syntax for data structures:

Python:

    a_list = [1,2,3]
    a_dictionary = {'a':1,'b':2,'c':3}
    a_tuple = (1, 2, 3)

Java:

    List<Integer> aList = Arrays.asList(1, 2, 3);
    Map<String,Integer> map = new HashMap<>() {{
        put("a", 1);
        put("b", 2);
        put("c", 3);
    }};
    List<Integer> aTuple = com.google.common.collect.ImmutableList.of(1, 2, 3);

Java really doesn't have good support for literal data structures, which aren't used very frequently in practice.
 
### Lists
 
List operations:

Python:

    items = ['a','b','c']
    print(items[1])  # b
    items[1] = 'd'
    items.append('e')
    items += 'f'  # same as append
    print(items)  # ['a', 'd', 'c', 'e', 'f']

Java:

    List<String> items = Lists.newArrayList("a", "b", "c");
    System.out.println(items.get(1));
    items.set(1, "d");
    items.append("e");
    items.append("f");
    System.out.println(items);

Iterating a list:

Python:

    for item in [1,2,3]:
        print(item)

Java:

    List<Integer> items = Lists.newArrayList(1, 2, 3);
    for (Integer item : items) {
        System.out.println(item);
    }
 
Iterating a list with index:

Python:

    for index, item in enumerate([1,2,3]):
        print(item + ' at position ' + index)
 
Java:

    List<Integer> items = Lists.newArrayList(1, 2, 3);
    for (int i = 0; i < items.size(); i++)) {
        System.out.println(items.get(i) + " at position " + i);
    }

Contains is done with the in keyword:

Python:

    items = [1, 2, 3]
    print(1 in items)

Java:

    List<Integer> items = Lists.newArrayList(1, 2, 3);
    System.out.println(items.contains(1));
 
The size of a list is calculated with the len() built-in fuction:

Python:

    items = [1, 2, 3]
    print(str(len(items)))  # 3

Java:

    List<Integer> items = Lists.newArrayList(1, 2, 3);
    System.out.println(items.size());
 
### Dictionaries
 
Operations on dictionary:

Python:

    my_dict = {'a':'x','b':'y','c':'z'}
    my_dict['d'] = 'w'
    print(my_dict['d'])
    print(my_dict.get('d')
    print(my_dict.get('e','some other string'))  # default value if key doesn't exist
 
Java:

    Map<String,String> my_dict = new HashMap<>(){{
        put("a", "x"); put("b", "y"); put("c", "z");
    }}
    
    my_dict.put("d", "w");
    System.out.println(my_dict.get("d"))
    System.out.println(my_dict.contains("e") ? my_dict.get("e") : "some other string")
 
Iterating a dictionary:

Python:

    my_dict = {'a':'x','b':'y','c':'z'} 
    
    for key in my_dict:
        print(key + "=>" + my_dict[key])
     
    for key, value in my_dict.items():  # destructures key/value pairs into tuple, and assigns to two variables
        print(key + "=>" + value)
 
Java:

    Map<String,String> my_dict = new HashMap<>(){{
        put("a", "x"); put("b", "y"); put("c", "z");
    }}
    
    for (String key: my_dict.keySet()) {
        System.out.println(key + "=>" + my_dict.get(key));
    }
    
    for (Map.Entry<> entry: my_dict.entrySet()) {
        System.out.println(entry.getKey() + "=>" + entry.getValue());
    }
 
One common thing that dictionaries are used for is counting, and you usually have to do the "if key doesn't exist set to 0 else increment".  defaultdict means you don't need to do that -- it creates a dictionary that defaults all of the key values when they're get to it's param:

Python:

    import collections
    my_dict = {}
    print(my_dict['some_key'])  #KeyError!
    my_dict = collections.defaultdict(int) # or string, float, list, dict, tuple, set
    print(my_dict['some_key'] is not None)
    my_dict['some_key'] += 1
    print(my_dict['some_key'])  # 1
 
### Tuples
 
Used for lightweight data structures. Immutable. There are a few "Pair" classes out there, but not really an idiomatic Java data structure.

    a_tuple  =  ("a", "b", "c")
    print(a_tuple)
    shows = []
    shows.append(("a", "b", "c"))  # must use append, b/c += adds each element of the tuple
    (first, second, third) = shows[0]  # destructure tuple into three variables
    print(first + " " + second + " " + third)
 
Strings
-------

Strings are immutable.
 
The canonical way to create a string of output is to put each piece into a list and then put it all together at once (e.g., basically what Java's StringBuilder does)

Python:

    output = []
    output += 'foo'
    output += 'bar'
    print(''.join(output))

Java:

    StringBuilder output = new StringBuilder();
    output.append("foo");
    output.append("bar");
    System.out.println(output.toString());

 
replace - returns a new string, as in Java

    print('foo'.replace('f', 'z'))  # zoo
 
split - returns a list (rather than a String array as in Java)

    print('foo bar baz'.split(' '))  # ['foo', 'bar', 'baz']
 
strip
 
    print("'" + ' foo '.strip() + "'")  # no spaces
    print("'" + ' foo '.rstrip() + "'")  # only leading spaces
    print("'" + ' foo '.lstrip() + "'")  # only trailing spaces

Regexes
-------

Two ways to use: function with regex passe, or pattern object

https://pythex.org/ is great for testing them.
 
    import re
 
    regex = re.compile('(\w+) (\d+) (\w+) (\d+)')
    line = 'ABC 123 XYZ 789'
    match = regex.search(line)
    if match:
        print("group(0): " + match.group(0))
        print("group(1): " + match.group(1))
        for m in match.groups():
            print("from groups > " + m)
           
    
re.match(pattern, string [, flags]) # from start of string only.
re.search(pattern, string [, flags]) # anywhere in the string, but only one match
re.findall/finditer(pattern, string [, flags]) # anywhere in the string, all matches

re.split(pattern, string)
re.sub/subn(pattern, replacement, string)
 
pattern = re.compile(pattern [, flags])
pattern.findall(mystring)
 
Useful flags: 
re.DOTALL - '.' also includes a newline
re.MULTILINE - match multiline strings
 
Templates
---------

There are a few built-in ways of doing simple string substitution:
 
    print("i am a %s" % 'python string')
    print("i am a {0}".format('another python string'))
    print("with %(kwarg)"%{'kwarg':'python string'})
    print("with {kwarg}".format(kwarg='another python string'))
 
The Template class comes in handy for more complex ones:

    from string import Template
     
    create_template = Template('''
    {
        "parent": "$parent",
        "subject": "$subject",
    }
    ''')
     
    create_json = create_template.substitute({'parent': 'some url', 'subject': 'a subject'})
    
    print(create_json)
 
 
Requests
--------

Python's built-in network support in urllib is very low level and difficult to use.  To the rescue is [Requests: HTTP for Humans](http://docs.python-requests.org/en/latest/). 
 
It's this easy:

    import requests
    r = requests.get('http://www.google.com')
    print(r.text)
 
You can also easily add query params and get the resulting status code:

    import requests
 
    r = requests.get('http://www.google.com', params={'q': 'cat gifs'})
    print('Body: ' + r.text[:256])
    print('Status Code: ' + str(r.status_code))
 
In addition to the get function, there's also post.  Basic Authentication credentials can be passed with the auth named parameter and data passes post data.  If the response data is JSON, the json() function parses and returns a dictionary representation of the data.
 
    import requests
    import json
     
    post_uri = ...
     
    username = input("username >")
    password = input("password >")
     
    create_doc_json = '''
    {
        "type": "document",
        "content":
          {
          "type": "text/html",
          "text": "'the body of my document"
          },
        "subject": "the subject"
    }
    '''
     
    r = requests.post(post_uri,
                      auth=(username, password),
                      data=json.dumps(create_doc_json))
     
    print(r.json())

XML
--------

[lxml](http://lxml.de/)

   from lxml import html
   page = requests.get('http://econpy.pythonanywhere.com/ex/001.html')
   tree = html.fromstring(page.text)
   
   buyers = tree.xpath('//div[@title="buyer-name"]/text()')
   

   >>> from lxml.cssselect import CSSSelector
   >>> sel = CSSSelector('div.content')
   >>> sel  #doctest: +ELLIPSIS
   <CSSSelector ... for 'div.content'>
   >>> sel.css
   'div.content'

   from lxml.etree import fromstring
   >>> h = fromstring('''<div id="outer">
   ...   <div id="inner" class="content body">
   ...       text
   ...   </div></div>''')
   >>> [e.get('id') for e in sel(h)]
   ['inner']

    h.cssselect('div.content') == sel(h)
 
JSON
----

The json module has some functions convenient to handling JSON data.  The core idea behind them is it's natural to manipulate Python dictionaries, lists, and primitive types and the convert them to and from a JSON string representation.  The json.loads and json.dumps functions do this, converting a string to a dictionary and vice versa. The json.load and json.dump functions operate on streams, so you can convert to and from without the intermediate string representation in memory.
import json
 
    json_str = '''
    {
        "foo" : {
            "attr1" : "bar"
        }
    }
    '''
 
    json_dict = json.loads(json_str)
    print(json_dict['foo']['attr1'])  # bar
    json_dict['foo']['attr1'] = "baz"
    print(json.dumps(json_dict))  # {"foo": {"attr1": "baz"}}
 
[19.2. json — JSON encoder and decoder — Python 3.4.0 documentation](http://docs.python.org/3/library/json.html)
 
Running external commands
-------------------------

Python is a fantastic alternative to shell scripts, and running external commands is essential to that:

    import os
    os.system('curl http://news.ycombinator.com -o hackernews.html')

the popen() function can also be used.

[16.1. os — Miscellaneous operating system interfaces — Python 3.4.0 documentation](http://docs.python.org/3/library/os.html)
 
Function Definition
-------------------

Functions are available to be used after they're defined in a file.
 
Functions are defined as follows:

    def my_function(param1='a', param2='b'):
        print(param1 + ' ' + param2)
 
Default values can be assigned to any parameters, which makes including values for these parameters in function calls optional.
 
Functions can be invoked with parameters either positionally or named, which leads to a lot of flexibility without needing to define many different functions with varying parameter lists that simply call another function with more parameters.

    def my_function(param1='a', param2='b'):
        print(param1 + " " + param2)
 
    my_function()
    my_function('c')
    my_function('c', 'd')
    my_function(param1='e', param2='f')
    my_function(param2='g', param1='h')
 
Sorting
-------

In place sorting can be done in-place on a list with sort(). sort does not return the value of the sorted list, so be careful not to assign it to a variable.

    nums = [3, 2, 4, 1, 5]
    nums.sort()
    print(nums)  # [1, 2, 3, 4, 5]
 
For more complex types of sorts, the sorted built-in is used.  For example, if we have a list of tuples, we can pass a lambda (anonymous function) that takes the value of the tuple, and returns the second value in the tuple.  sorted also has an optional parameter reverse which defines which order the sorting should be done it.
 
    items = [(3, 'a', 3.4), (2, 'b', 1.2), (4, 'c', 5.3)]
    sorted_items = sorted(items, key=lambda x: x[1], reverse=True)
    print(sorted_items)
 
Comprehensions
--------------

Comprehensions are a fancy name for creating one list from another list by defining what should be in the new list.
 
For example, if we had a list of tuples containing three values, we may want to get a list of only the first element of the tuple only when the second element matches some criteria.
    
    items = [(3, 'foo', 3.4), (2, 'bar', 1.2), (4, 'baz', 5.3)]
    print([item[0] for item in items if item[1].startswith('b')])
 
So we're basically saying:
 
<the value to put in the list> for <the variable name for an item in the list> in <the initial list> if <the criteria the item must match>
 
Each of these items can be any expression, so it's tremendously powerful for processing.

Time
----

    import time
 
    start_time = time.time()  #floating point seconds since the epoch
    time.sleep(1)
    print('%.2f' % (time.time() - start_time) + 's')
 
    import time
    print(time.strftime("%a, %d %b %Y %H:%M:%S"))  # e.g., Thu, 27 Mar 2014 20:31:25


Math

// floored division
** exponentiation

math lib
numpy
augmented assignment, no ++ --
link to list of math functions

Next Steps

* [The Hitchhiker’s Guide to Python!](http://docs.python-guide.org/en/latest/)
* Learning Python by Mark Lutz
* Python Cookbook by David Beazley and Brian K. Jones
* Python for Data Analysis by Wes McKinney
* [Mouse vs. Python](http://www.blog.pythonlibrary.org/)
* http://python4java.necaiseweb.org/Main/TableOfContents
* http://interactivepython.org/courselib/static/java4python
* http://home.wlu.edu/~lambertk/pythontojava/
 
