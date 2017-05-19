# A novel approach to graphical user interface development using automatic code generation from images

This is the Bachelor's thesis for Einar Johnsen and Christopher Dambakk, University of Agder, spring 2017. Graphical user interface (GUI) development is a time consuming process. This thesis shows that graphical user interfaces can automatically be generated based on images. The automation of GUI development lets the developer spend valuable time creating improved user interfaces and user experiences. The thesis results in a rule based prototype converting images of GUIs into structures, and structures into source code targeting HTML and iOS applications. The prototype supports user defined GUI elements and can be extended to target even more platforms. Hopefully, the prototype becomes an essential and helpful product in the daily life of a developer.

**Authors:**

Einar Johnsen	

Christopher Dambakk

**Supervisor:**

[Morten Goodwin, Ph.D.](http://www.mortengoodwin.net/)

**Contractor:**

[Agens AS](https://www.agens.no/)

**Tool usage documentation further down**

Table of contents
=============
- [Introduction](#introduction)
	- [Primary goals](#primary-goals)
	- [Secondary goals](#secondary-goals)
	- [Tertiary goals](#tertiary-goals)
- [Tool usage documentation](#tool-usage-documentation)
	- [Tool dependencies](#dependencies)
	- [The input image](#the-input-image)
	- [The JSON structure](#the-json-structure)
	- [The configuration file](#the-configuration-file)
	- [Image Parser tool documentation](#the-image-parser)
	- [HTML generator tool documentation](#the-html-generator)
	- [iOS generator tool documentation](#the-ios-generator)
		- [Xcode hacks](#run-the-app-with-xcode)


Introduction
============

This project presents an alternative approach streamlining the time consuming process of developing graphical user interfaces (GUI). The presented prototype makes runnable GUI source code targeting different platforms based on a corresponding image. A programmatic approach to understand the intended GUI representation from an image is far from trivial. The images used in this project are idealized representations of user interfaces where the graphical components are marked by colored rectangles. The rectangle color is mapped to predefined, platform-specific graphical components. When reading the images, the information extractable is stored in a structural manner to be interchangeable between internal and external tools.

The problem is defined by the contractor Agens. Agens is an app development company stationed in Grimstad and Oslo who has a major activity in app development and they see a room for improvement when it comes to the way GUIs are created. According to Agens, many user interfaces in mobile applications are essentially the same with only minor differences in use of graphical elements, but are nevertheless time consuming to develop. In addition, app development is already heavily characterized by automation with the use of templates. However, the GUIs created by templates must often be altered to be adapted to the user's requirements. This project aims to give developers a new way of creating GUIs in a fast and easy way.

Agens have defined a list of goals and requirements relevant to produce a complete project and other interesting tasks to solve. The requirements in section Primary goals are essential and should be complete to consider this project a success. 

### Primary goals

-   Create a simple, extendable definition for all common UI elements.

-   Create a command line interface (CLI) tool that reads png images and pairs any app design contained to a structured representation.

-   Create a CLI transpiler to a selected platform. (e.g. iOS, Android, HTML, etc).

-   Write documentation for tool usage.

-   Support different levels of output logging and offer reasonable error messages.


### Secondary goals

-   Create a tool that draws a representation of the semantic representation for the sake of manually being able to verify the structure that is being created.

-   Extend the solution to handle more than one target platform and as many UI elements as possible.

-   It should be possible to run the tool chain multiple times without destroying work that has been manually done. It should also be possible for the designer to update the drawings and add/change UI elements and add those to the project in a non destructive manner.

- Code that is generated should follow good naming conventions and avoid uninformative names, like label\_1 or textbox3 etc. This will require some sort of analysis or meta information solution.

### Tertiary goals

- Extend the image parser and transpilers to include application navigation flow.

- Extend the image parser to consider meta information. Examples of meta information could be contextual names (e.g. user name, phone number, etc) or composite or custom UI elements.

- Extend the tools to support user defined elements.

- Extend the transpiler to consider meta information such as API endpoints, namespaces and libraries.

- Recommend features and tools that can extend the tool chain.

- Extend the image parser to work directly on the designers sketch, without the designer marking the UI elements.

- Set the tool chain up as a private tap for use with the OS X package manager homebrew. 



Tool usage documentation
===============

## Dependencies


The tools use the following external libraries:

### Image Parser:
- argparse - https://docs.python.org/3/library/argparse.html
- json - https://docs.python.org/3/library/json.html
- PIL - https://pypi.python.org/pypi/PIL
- colorama - https://pypi.python.org/pypi/colorama
- configparser - https://docs.python.org/3/library/configparser.html
- collections - https://docs.python.org/3/library/collections.html
- time - https://docs.python.org/3/library/time.html


### HTML generation tool:
- argparse - https://docs.python.org/3/library/argparse.html
- json - https://docs.python.org/3/library/json.html
- configparser - https://docs.python.org/3/library/configparser.html
- collections - https://docs.python.org/3/library/collections.html
- BeautifulSoup ver 4 - https://pypi.python.org/pypi/beautifulsoup4


### iOS generation tool:
- argparse - https://docs.python.org/3/library/argparse.html
- colorama - https://pypi.python.org/pypi/colorama
- configparser - https://docs.python.org/3/library/configparser.html
- collections - https://docs.python.org/3/library/collections.html
- json - https://docs.python.org/3/library/json.html
- GitPython - https://pypi.python.org/pypi/GitPython/
- re - https://docs.python.org/3/library/re.html 
- readline - https://docs.python.org/3/library/readline.html
- time - https://docs.python.org/3/library/time.html
- glob - https://docs.python.org/3/library/glob.html
- copy_tree distutils.dir_util - https://docs.python.org/3/library/distutils.html
- shutil - https://docs.python.org/3/library/shutil.html



## The input image

The input image must be standardized for the parser to understand it. Follow these rules:

- Must be a png file.
- Anti aliasing must be turned off when designing the image.
- For a best possible result, match the image size with the size of the desired result.
- Colored rectangles represent the different graphical components. Each color represent a kind of component.

Image (4) is an example of how the input image might look like. 

![Example image](RealWorldExample2.1.png) 


## The JSON structure

The created JSON structure is a list of elements where an element is a list with attributes. An example element can look like this:

```javascript
{
	"id": 0, 
	"parent": -1, 
	"parentColor": "", 
	"color": "#c80005", 
	"x": 69, 
	"y": 23, 
	"relX": 69, 
	"relY": 23, 
	"width": 117, 
	"height": 98, 
	"content": {}
}
```

Take a look at the [demo-file](demo.json) for a complete example. That is the JSON structure for [this image](demo.png).




## The configuration file

The configuration file containts generic description of all graphical components. These will be used to replace the colored rectangles in the images. 

### Configuration in HTML

The configuration file in HTML is simple. Here is a example that defines the div-tag in html:

```html
[tags]
0000ff:div

[html-elements]
0000ff : <div class="CSSCLASS">CONTENT</div>
```

This assigns the collor #0000ff to the div tag. The CSSCLASS and CONTENT will be replaced with the corresponding CSS-class created and child elements of the div, if any. 

### Configuration in iOS

The configuration file in iOS is a bit more complex due to the more complex file structure than HTML. Consider this example:

```
[implementationsFields]
c80005 : [["view"],["self.NAME = [[UIView alloc] initWithFrame:CGRectMake(POSX, POSY, WIDTH, HEIGHT)];"], ["self.NAME.backgroundColor = [UIColor COLOR];"]]

[addingFields]
default : ["[self.view addSubview:self.NAME];"]

[headerFields]
c80005 : [["view"], ["@property (nonatomic, strong) UIView *NAME;"]]
```

This is how the view component in iOS is defined. Each element needs the "implementation"- "adding"-, and "header"-part. 


## The Image Parser

To run the Image Parser, open a console and enter the following:

`python3 PATH_TO_PARSER/imageParser.py PATH_TO_IMAGE PATH_TO_OUTPUT`

where

- `PATH_TO_PARSER` is the location of the Image Parser on your computer
- `PATH_TO_IMAGE` is the complete filepath to the image you want to convert to JSON
- `PATH_TO_OUTPUT` is the path to the desired location for the output JSON file representing the view.

You can also add the verbose flag ( `-v` eller `--verbose`) for more verbose feedback to the console.


## The HTML generator

To run the HTML generation tool, open a console and enter the following:

`python3 PATH_TO_HTMLGEN/htmlGenerator.py PATH_TO_JSON TITLE PATH_TO_OUTPUT`

where

- `PATH_TO_HTMLGEN` is the location of the HTML generator on your computer.
- `PATH_TO_JSON` is the complete filepath to the JSON file containing the description of the view. This JSON file will typically be the output of the Image Parser-tool. E.g. `/home/username/documents/myFirstView.json`
- `TITLE` is the title of the web page
- PATH_TO_OUTPUT` is the path to the desired location for the generated HTML and CSS file



## The iOS generator

To run the iOS generator open a console and enter the following:
`python3 PATH_TO_IOSGEN/iosgenerator.py PATH_TO_JSON PATH_TO_OUTPUT`
where
- `PATH_TO_IOSGEN` is the location of the iosgenerator on your computer.
- `PATH_TO_JSON` is the complete filepath to the JSON file containing the description of the view. This JSON file will typically be the output of the Image Parser-tool. E.g. `/home/username/documents/myFirstView.json`
- `PATH_TO_OUTPUT` is the path to desired place to put the created project or the path to the project to be updated. The last directory will be the folder containing the app and will be the name of the app. E.g. `/home/dev/apps/MyFirstApp` where MyFirstApp is the name of the app.

You can also add one or more of the following flags:
- `-v` for more verbose output logging to the console.
- `-f` is the force flag to make the script continue even if it may cause unexpected behaviour and results. Without the flag, the program will stop and tell the user to add the flag if it encounter any problems.

The script will ask for input during runtime, such as enter a name for the view, select a view or commit changes using git.
For further help, use `--help` or `-h` when running the tool.

### Run the app with Xcode

To be able to view the files in Xcode you must follow these steps:

- In the file structure overview in Xcode, delete the ViewController- and ViewControllerBase files.
- Add the newly created views by clicking "File" >> "Add files to ..." >> Select the newly created files >> "Add"


If you cannot build the application, follow these steps:

"Product" >> "Scheme" >> "Edit Scheme" >> Executable: ProjectName.app