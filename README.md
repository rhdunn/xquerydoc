#xquerydoc

Parses xqDoc comments from your xquery and generates a set of API
level documentation implemented in pure XQuery v1.0.

The following versions of XQuery are supported.

  * XQuery v1.0 
  * XQuery 1.0-ml
  * XQuery v3.0 

xquerydoc commandline uses XMLCalabash (which ships with Saxon) though 
as xquerydoc is implemented in pure XQuery v1.0 you may also invoke
from most XQuery processors (Saxon, MarkLogic, XQilla, eXist ...).


##Install

To use from the commandline you must have JVM and XML Calabash XProc
processor installed.

To install XML Calabash use the included installer at deps/calabash-0.9.34.jar.

```bash
java -jar calabash-0.9.34.jar.
```

Alternately you may download latest XML Calabash from
http://xmlcalabash.org

xquerydoc can be invoked directly from any XQuery v1.0 compliant
processor but you will have to take care of applying styling to xqDoc
markup.

##Usage

There are several ways to use xquerydoc.

###Invoking xquerydoc from the commandline

The *bin/xquerydoc* script can be invoked from the commandline and
generate documentation from xquery containing xqdoc comments. This
uses XML Calabash XProc pipeline (which comes with SAXON XQuery
processor) to invoke XQuery v1.0 xquerydoc and generate documentation
by applying XSLT transformation.

To use you simply point xquerydoc to a directory or single xquery file
as well as provide a directory for the documentation to be generated
into to.

```bash
xquerydoc {xquerydoc} {output html}
```

The following example will process a directory containing xquery and
output documentation to another directory.

```bash
xquerydoc /some/directory/containing/xquery/ /output/html/to/some/directory/
```

In addition we provide a MarkLogic variant of this script which will
connect to MarkLogic server (via XDBC) and invoke the MarkLogic v1.0
xquerydoc and generate documentation.

```
ml-xquerydoc /some/directory/containing/xquery/ /output/html/to/some/directory/
```

To use this variant you will need to setup MarkLogic XDBC server and
provide details in *etc/config.xml* file.

Note that you do not need to use ml-xquerydoc to genreate
documentation from a set of 1.0-ml version scripts, xquerydoc will do
that for you. 

###Invoking xquerydoc from xquery

As xquerydoc is itself written in pure XQuery v1.0  you can invoke  directly
from your own xquery applications employing the *xqdoc:xqdoc()* function to extract xqDoc comments.

Whilst xquerydoc itself is written in XQuery v1.0, as a convenience we have provided XQuery processor specific 
implementations to apply stying.

####XQuery v1.0 Example (Saxon)
```xquery
xquery version "1.0" encoding "UTF-8";

import module namespace xqdoc="http://github.com/xquery/xquerydoc" at "/xquery/xquerydoc.xq";

xqdoc:xqdoc(fn:collection('/some/xquery/?select=file.xqy;unparsed=yes')) 
```
As with the commandline version we provide for your convenience a
MarkLogic version (though the XQuery v1.0 should also run within
MarkLogic just as well). 

####MarkLogic Example
```xquery
xquery version "1.0-ml" encoding "UTF-8";

import module namespace xqdoc="http://github.com/xquery/xquerydoc" at "/xquery/ml-xquerydoc.xq";

xqdoc:xqdoc(xdmp:document-get(fn:concat($distpath,$example))) 
```

These examples show how to extract xqDoc comments with the xqdoc:xqdoc() function outputing xml as follows.

```xml
<doc:xqdoc xmlns:doc="http://www.xqdoc.org/1.0">
  <doc:control>
    <!--Generated by xquerydoc: http://github.com/xquery/xquerydoc-->
    <doc:date/>
    <doc:version>N/A
    </doc:version>
  </doc:control>
  <doc:module type="main">
    <doc:uri/>
    <doc:comment>
      <doc:description> &#xD;  This main module controls the
      presentation of the home page for&#xD;  xqDoc.  The home page
      will list all of the library and main modules&#xD;  contained in
      the 'xqDoc' collection.&#xD;  The mainline function invokes only
      the&#xD;  method to generate the HTML for the xqDoc home page.
      A parameter of type &#xD;  xs:boolean is passed to indicate
      whether links on the page should be constructed &#xD;  to static
      HTML pages (for off-line viewing) or to XQuery scripts for
dynamic&#xD;  real-time viewing.&#xD; &#xD;
      </doc:description>
      <doc:author> Darin McBeath&#xD;
      </doc:author>
      <doc:since> June 9, 2006&#xD;
      </doc:since>
      <doc:version> 1.3&#xD;
      </doc:version>
    </doc:comment>
  </doc:module>
  <doc:variables/>
  <doc:functions/>
</doc:xqdoc>
```

Its relatively easy to then take this XML and style it using provided
XSLT stylesheets provided for under *src/lib*. The Marklogic variant
XQuery library provides this in a utility function contained in *src/xquery/ml-utils.xqy*.


## API Docs

Yup we eat our own dog chow, view API docs here.

## Distro

* xquerydoc
  * bin
  * etc
  * docs
  * src
    * tests: contains tests
    * lib: contains xslt and associated javascript, css, etc
    * src: contains xquerydoc XQuery modules 
  * ebnf: contains Extended Backus–Naur Form definitions of XQuery language

##Running Tests

To run all tests

```bash
bin/run-all-tests.sh
```

All this script does is run the following xquery processor specific scripts.

```bash
bin/run-saxon-tests.sh 
bin/run-marklogic-tests.sh
```

Test scripts work by invoking an XProc pipeline in *src/tests* (either saxon-test.xpl or marklogic-test.xpl).

To run MarkLogic tests you will need to setup XDBC server and edit
*src/tests/config.xml* with relevant details.

If you want to invoke these scripts manually please review the test
run scripts to understand what needs to be passed into Calabash.

##Dependencies

For convenience we have included all the dependencies xquerydoc
requires.

  * XML Calabash (http://xmlcalabash.com- to install run > java -jar calabash-0.9.34.jar
  * Saxon XQuery and XSLT Processor by Michael Kay (http://www.saxonica.com)  ships with XML Calabash

Please review the licenses of all included software.

##Credit, Acknowledgements

Created by John Snelson, James Fuller 

XQuery parsers were generated from EBNF using Gunther Rademacher
excellent http://www.bottlecaps.de/rex/

Norman Walsh's XML Calabash is available under either the GPLv2 or Sun's CDDL license 

Saxon XQuery and XSLT 2.0 Processor by Michael Kay is released under Mozilla Public License

Prettify (used by api doc and testsuite) is released under Apache License, Version 2.0

XQuery prettify 'brush' was provided by Patrick Wied (as part of a MarkLogic
bounty contest ;) ) and can be obtained here http://www.patrick-wied.at/static/xquery/prettify/ 

Though there is no reuse of any code from the  original xqDoc project,
we have opted to use a subset of the xqDoc XML format, the codebase is released under Apache License, Version 2.0


## License

xquerydoc is released under Apache License v2.0

Copyright 2011 John Snelson, James Fuller

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and

##FAQ

*Why a pure XQuery v1.0 implementation ?"

This means you can generate api level documentation using just XQuery
! Need we say more ?

* Why use XML Calabash XProc ?*

As we are applying an XQuery process and an XSLT process on a set of
xquery documents it seemed like a good match for the commandline
invokation. You can of course use the XQuery v1.0 libraries without
XProc but you will need to apply XSLT stylesheets using your
processors capabilities.


##Contact


{TBD}









--------------------------------

Jim's Notes
-----------

* docs
  * embed xqdoc in xquerydoc own code and generate for /docs
  * create markdown version of README

* build based on xproc
  * upload EBNF (loop through all EBNF) to http://www.bottlecaps.de/rex/ 
  * download result and place in src/xquery
  
* test
  * test xquery1, xquery3, xquery-ml
  * create tests 

* xquery
  * create xquery entry point for parsing and generating output
  * create documentation targets for the following formats
    * html
    * text
    * markdown
    * docbook

* integrate existing xqdoc disply routines

* make work across several xquery processors

