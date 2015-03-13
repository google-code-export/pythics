Multiprocess Python framework for creating applications.

Pythics is an application for running Python code intended to be used for simple interfaces to laboratory instrument or numerical simulations. It features a simple system for making graphical user interfaces (GUIs), useful controls including plotting, clean separation between GUI and application code, and multithreading and multiprocessing so running backend code does not interfere with the functionality of the GUI. Pythics attempts to robustly handle all of the complex details of writing a program with a GUI for you, allowing you to concentrate on the functionality of your program.

Pythics is developed by the D'Urso research group at the University of Pittsburgh in the Department of Physics and Astronomy. Our homepage is at http://www.nanomaterials.phyast.pitt.edu/


---


## VERSION 0.6.1 ##

This release fixes several bugs that were left after major changes in the previous release.

  * Fixed error in qwt controls that was preventing parameter values from being set correctly from a program (mistyped self.block\_signals).

  * Fixed errors in documentation of matplotlib-based plots. The available actions were incorrectly described as e.g 'button\_press\_action' when they should have been e.g. 'button\_press\_event'. (Note: the action names are set by Qt and matplotlib, not by pythics.)

  * Removed deprecated methods pop\_event(), get\_events\_length(), and clear\_events() from mpl.Plot2D control. They had never been properly documented and were broken in the last release. You should use mpl.Plot2D.events to access event information in mpl.Plot2D. Other mpl controls have a similar attribute. Further information was added to the documentation for mpl controls.

  * Fixed error messages when using TextIOBox with a logger by adding a len method to TextIOBox which returns the number of lines.


---


## VERSION 0.6.0 ##

WARNING: UPGRADING TO PYTHICS 0.6.0 WILL REQUIRE CHANGES TO YOUR CODE (MOST LIKELY TO YOUR .HTM FILES)!

This release includes a major rewrite and significant refactoring. Much of the object proxy code was rewritten to generate proxies dynamically, building off the design of the old mpl.Canvas object. This design change has significantly simplified the coding of Pythics controls, decreased the total code in Pythics, and made it possible to use standard pyQt controls directly in Pythics, without additional wrapping code. It is now also possible to use the html layout widget independent of the rest of Pythics, or to use Pythics for inter-process communication with layouts other than the html widget. Examples of these should be included in future releases.

  * New documentation available through the help menu within Pythics. This is now the primary documentation. The pdf manual is no longer available, but it may be rebuilt in the future.

  * The way control actions are specified has changed for all controls. Instead of having multiple 'action' parameters, each control has an 'actions' dictionary with an entry for each action the control can trigger. See the help for each control for details.

  * All parameters in html now passed through eval() in Python. If an exception is raised, the parameter is interpreted as a string. For safety, all strings passed as html parameters should now be double-quoted. For example:



&lt;object classid='FilePicker' id='file\_picker' width='100%'&gt;


> 

&lt;param name='label' value="'Load file'"/&gt;


> 

&lt;param name='save' value='True'/&gt;


> 

&lt;param name='type' value="'open'"/&gt;




&lt;/object&gt;



  * Library import now handled with Import control.

  * mpl.Canvas no longer gives access to the matplotlib library as an attribute. Instead, use an Import control.

  * Callbacks triggered in SubWindows now get sent all controls as arguments, not just the controls in the subwindow.

  * ParameterLoader, ScriptLoader, GlobalNamespace, GlobalTrigger, GlobalAction all default to showing no widget.

  * mpl.Plot2D and mpl.Chart2D now have right-click menu to save figure to file.

  * The previously deprecated mpl.MPLPlot control is no longer supported. Use mpl.Canvas instead.

  * The examples directory was moved into the pythics directory and is now installed with the win32 installer as well as the .zip distribution.


---


## VERSION 0.5.2 ##

  * Fixed bug in mpl.Plot2D that made events unusable (although it is not documented yet anyway).

  * Fixed bug in mpl.Plot2D.set\_properties that made it unusable.

  * Added 'c\_limits' keyword argument to mpl.Plot2D new\_colormesh, new\_image, and set\_properties methods to set the colormap limits.

  * Properly remove old plot elements from Plot2D when they are deleted or replaced with new plot elements of the same name.

  * Updated fitting example to allow plotting of initial guess.


---


## VERSION 0.5.1 ##

  * Fixed long-standing bug in pythics use of QThread that could cause deadlocks in the GUI. This bug rarely impacted behavior on Windows XP, Windows 7, or Linux, but caused frequent locking of the GUI under Windows 8. The elimination of this bug is the primary reason for this release.

  * Catch unhandled exception which was raised when cancel was pressed after choosing File -> Open, File -> Open Workspace, File -> Save Workspace and File -> Save Workspace As... This was a long-standing issue, but it never caused a problem other than generating unnecessary error messages.

  * Added is\_set() method to EventButton which tells you whether the event has been triggered without having to give any wait time.


---


## VERSION 0.5.0 ##

After a short 0.4.x series, this release number bumps up to 0.5.0 because of incompatible changes which will minimize the burden of some old controls (e.g. Plot) without removing the controls entirely. The pyQwt controls must now be specified as e.g. qwt.Plot and are not loaded unless used. MPLPlot has been replaced by mpl.Canvas and is now only available as deprecated.MPLPlot. See details below. With this reorganization, add-on libraries (PyQwt and matplotlib) are only loaded if a control they provide is used, making them optional.

  * New control mpl.Canvas:

> There is a new control, 'mpl.Canvas', which aims to wrap essentially the complete functionality of the matplotlib library. It is intended to replace MPLPlot, which will eventually be eliminated. Note that mpl.Chart2D and mpl.Plot2D are much easier to use and likely faster than mpl.Canvas, but they do not give access to the full capabilities of matplotlib. The mpl.Canvas control also automatically adapts to future changes in or additions to matplotlib. We anticipate that mpl.Canvas, mpl.Plot2D, mpl.Chart2D, and (future) mpl.Plot3D will be the long-term supported plotting controls.

  * Code reorganization: MPLPlot moved to deprecated.MPLPlot

> Since mpl.Canvas is intended to replace MPLPlot, MPLPlot is now only available as deprecated.MPLPlot.

  * Code reorganization: pyQwt-based controls moved to qwt.control\_name

> This change was made because we no longer use pyQwt as our primary plotting engine. While pyQwt may fade away since it has no python3 port and it doesn't appear to be an active project, we will keep these controls around as long as it is practical. PyQwt is now considered optional and is only loaded if one of the qwt controls is used.

  * Most code will need to be changed to run under Pythics 0.5.0. Here are the required substitutions of the control classid in your html files:

> - MPLPlot -> deprecated.MPLPlot

> - Plot2D -> mpl.Plot2D

> - Chart2D -> mpl.Chart2D

> - Dial -> qwt.Dial

> - Gauge -> qwt.Gauge

> - Knob -> qwt.Knob

> - Slider -> qwt.Slider

> - Chart -> qwt.Chart

> - Plot -> qwt.Plot

> - PointLinePlot -> qwt.PointLinePlot

  * Updates to the documentation.


---


## VERSION 0.4.1 ##

  * Changes to Chart2D control:

> - New html parameter memory='circular' or 'growable' (now used in chart\_recorder example). See Chart2D documentation.

> - Changed name of html parameter 'history' to 'length' to match the way the length of a circular or growable array is specified in Plot2D.

  * Control proxies now copy the original call signatures of the control method call, instead of appearing as `(*args, **kwargs)`. This gives clearer automatically generated documentation and better error detection in user code.

  * Changed filenames and paths in pythics-run.py from ascii to UTF-8 encoding to handle files and paths with non-ascii characters.

  * Changes in many controls:

> Convert all QStrings to UTF-8 before passing to user and removed explicit conversion of inputs to str by several controls. This should allow Pythics to handle unicode strings in most places.

  * Added encoding identification string at the top of each pythics source file.

  * Fixed warnings in ODE\_particle\_in\_box and ODE\_particle\_in\_circle simulation examples.

  * Updated init.py file to make only pythics.lib imported if you use `from pythics import *`, since that is the only file that a user would be intended to use.


---


## VERSION 0.4.0: ##

This release is significant in that essentially all plotting functionality is now available in matplotlib-based controls (Plot2D and Chart2D). Despite matplotlib's reputation as being too slow for real-time plotting, we found that by making heavy use of blit() and minimizing the redrawing of axes, matplotlib can actually be quite fast. We are interested in getting feedback about the use of matplotlib - does anyone find it to be unacceptable?

  * New plot control: Chart2D
> Chart2D, which uses matplotlib, is a replacement for the old Chart control, which used pyQwt. Its interface is a combination of Chart and Plot2D, so minor code changes are required to switch from Chart to Chart2D. Chart2D uses some tricks (like optionally hiding the axes labels while scrolling) to get reasonable plotting speed in most cases.
> NOTE: While there are no immediate plans to remove the old pyQwt plotting controls (Plot, Chart, PointLinePlot), new code should use the matplotlib-based plots: Plot2D and Chart2D.

  * Changes to Plot2D control:

> - Implemented Polar line/scatter plots; set html parameter 'projection' to 'polar' for polar plots.

> - Added colormesh plot item, which can be created with new\_colormesh(). This is similar to matplotlib's pcolormesh(), and it works in polar projections.

> - Added save\_figure() method to allow saving an image of the plot on command (an image can also be saved by right-clicking in in the plot in Plot2D and Chart2D).

> - Improvements to autoscale in animated plots.

  * New control EventButton: A combination of a button and a timer that is particularly helpful for making a data acquisition loop that runs with a fixed time interval and can be interrupted at any time during the delay, so it works well for both short and long time steps. See the new chart\_recorder example.

  * New objects GrowableArray, CircularArray, AppendBuffer (in pythics.lib). These currently have minimal or no documentation.

  * The chart\_recorder example has been rewritten (the old one is renamed chart\_recorder\_old). The new version uses the new chart control (Chart2D), a GrowableArray to store data easily, and an EventButton to control the timing and stop the data acquisition loop. The end result is that the new version is significantly simpler and cleaner than the old version.

  * Added a large number of new examples under examples -> simulations. These are simulations that I wrote for my computational physics class. They are nice demonstrations of Plot2D and of Pythics in general. ODE\_N\_particles is especially fun!

  * Fixed draw glitches that occur when scrolling animated matplotlib widgets by forcing a full blit when the scroll bar is released.

  * Removed old app.py file that should have been removed from the previous version. You should now start pythics with pythics-run.py.


---


## VERSION 0.3.9: ##

  * You can now start pythics from the command line as:

> Usage: pythics-run.py `[`options`]`

> Options:

> -h | --help    show help text then exit

> -a | --app     selects startup html file

> -v | --verbose selects verbose mode

> -d | --debug   selects debug mode

> The default logging level has now been changed to WARNING, so pythics prints fewer messages. Use the --verbose option to get the old level of logging (INFO) or --debug to get lots of debugging messages. This functionality is based on a patch submitted by Thomas Fechner.

  * New plot control: Plot2D, a simple to use plotting control which uses matplotlib. Other similar new plotting controls will be added in the future, eventually removing our dependence on pyQwt which is no longer being updated. This plotting control is actually fairly well documented in the docs.

  * Fixed bug in NumBox scientific notation that often made small values get rounded to zero when set.

  * Fixed long-standing bug that caused vi backends processes to continue unterminated if the vi was closed using the close box on its tab instead of File -> close.

  * Fixed bug in ImageButton where setting the value from Python code did not update the image on the button. E.g., now image\_button.value = True will change the button state and update the image.


---


## VERSION 0.3.8: ##

  * Lists passed as html parameters now need to evaluate properly as Python expressions. These are used in 'ChoiceBox', 'ChoiceButton', and 'RadioButtonBox' controls. If you need to use quotes inside the expression, use double and single quotes, e.g. the old way to pass a list was: `<param name='choices' value='[zero, one, two, three, four]'/>` while the new way is: `<param name='choices' value="['zero', 'one', 'two', 'three', 'four']"/>`.

  * Changes to MPLPlot control:

> - imshow method now allows updates, and imshow\_with updates has been deprecated.

> - update\_image\_data has been renamed update\_imshow\_data.

> - New methods pcolor and update\_pcolor\_data analogous to imshow and update\_imshow\_data, but in polar coordinates.

> - Added method colorbar to add a color scale indicator to image plots.

  * 'NumBox' controls can now work with scientific notation, e.g. 3.4e-12. Set the html parameter 'notation' to 'scientific' and change 'format\_str' html parameter if different formatting is desired.

  * Added 'get\_directory' method to FileDialog to ask user to choose a directory instead of a file, and changed names of 'open' and 'save' methods of FileDialog to 'get\_open' and 'get\_save'.

  * Added keyboard shortcut 'Ctrl+R' for File -> Reload.

  * 'save' html parameter is now accessible from the Python side as a property.

  * Added new html parameter 'user' to all controls that can be set to an arbitrary Python expression, then read as a control property. Note that it must be a string in html and must not raise an exception when passed to eval(). If you need to use quotes inside the expression, use double and single quotes, e.g.: `<param name='user' value="{'key1':1, 'key2':2}"/>` is ok. This is useful for tagging controls with information you may want to access from the Python side.


---


## VERSION 0.3.7 ##

  * Added confirmation dialog boxes to menu close, menu reload, and close boxes.

  * Added 'auto\_update' parameter to TextIOBox control. If set to True, the display is automatically updated after any change to the text (default value is True).

  * The PointPlot control has been renamed to PointLinePlot.

  * Added more html parameters to Plot and PointLinePlot. All options previously available only as set\_plot\_properties() arguments can now be used as html parameters.

  * Added draw\_points and draw\_lines methods to PointLinePlot to draw a collection of points or lines in one call. Added change\_point\_data, change\_points\_data, change\_line\_data, change\_points\_data, change\_point\_properties, change\_line\_properties, delete\_points, delete\_lines methods to manipulate plotted curves (which have to be named with the key argument).

  * New control, Shell, which is a Python console which can be embedded as a standard control in your vi. The console itself actually runs in the backend (action) process, and can be given access to all the variable and objects in your backend process.

  * TextIOBox control will now scroll to the bottom if reverse is not set to True to keep the newest information showing when used as a log.


---


## Version 0.3.6 ##

  * GlobalTrigger controls now have an 'action\_id' parameter which must match the GlobaAction 'id' instead of matching their id attributes.

  * Fixed Chart control grids so default is back to no grid, and fixed potential problem with grids and multiple calls to set\_plot\_properties.

  * Added x\_grid, y\_grid, dashed\_grid to set\_plot\_properties options in Plot and PointPlot.

  * Added grid test cases to Plot, Chart, and PointPlot in demo.

  * Manually tweaked sizing of Button with toggle to match size of regular button.

  * Added 'label' parameter to more controls that just printed some text: FileDialog and MessageDialog.

  * Made label-only widgets totally invisible if label is the empty string.


---


## Version 0.3.3 ##

  * Added 'require\_retrigger', and 'retrigger\_timeout' keyword arguments to Timer.start(). If require\_retrigger=True, then you must call Timer.retrigger() before the Timer will call your action again. This is to ensure that your action process does not fall behind the Timer action requests. If you do not call Timer.retrigger(), the Timer will retrigger after time retrigger\_timeout, which defaults to None (never retrigger). You can check if the Timer was  ever actually waiting for a retrigger with the new Timer.delayed attribute. It will be True if the Timer had to wait for a retrigger, and False otherwise.

  * Limited the number of pending GUI calls that any action process can queue to 2. Any further calls will wait to return until queue space is freed up. This eliminates one way pythics could get stuck if many more GUI requests were made then could be processed. GUI calls can no longer be passed the keyword argument 'asynchronous', as the default behavior should now always be reasonable.

  * Added 'rowspan' option to html table layout (contributed by Ilana Gat).

  * Added 'y\_grid', 'x\_grid', and 'dashed\_grid' properties to Chart control (contributed by Ilana Gat).

  * Fixed bug in proxy getattr methods.

  * Added 'severity' html parameter and property to MessageDialog control which changes the icon shown in the message box (with help from Ilana Gat).

  * Added 'message' property to MessageDialog control so the displayed message can be changed in python code.

  * Added 'directory' option to FilePicker control 'type' parameter to choose a directory (with help from Ilana Gat).

  * Added 'label' parameter to controls that just printed some text: ScriptLoader, ParameterLoader, GlobalNamespace, GlobalAction, GlobalTrigger, and Timer.

  * Fixed bug in workspace saving and wrong filename extension when opening a workspace.

  * Fixed minor bug that reported and error when the last tab was closed because the main window title could not be updated

  * imshow\_with\_updates and update\_image\_data in matplotlib to make fast updates of image plots practical.


---


## Version 0.3.0 ##

  * Fixed bug where window title was not updated when a tab was selected.

  * Fixed toggle button sizing bug.

  * NumBox no longer triggers value changed action until enter button is pressed or it loses focus.

  * Now possible to dynamically update choices in ChoiceBox.

  * Fixed bug in Gauge range set method.

  * Minor change in html.py so width of controls in % can be floats.

  * Removed old wx import from libinstruments.py.

  * Some new and updated instrument drivers.

  * new control: PointPlot, ultra-simple plotting of xy pairs.

  * new control: Plot, faster plotting of general data.


---


## Version 0.2.0: Port to PyQt, Major Changes ##

  * This release is port of pythics to the PyQt toolkit. The installation requirements have changed.

  * I also took the opportunity to clean up a lot of the API, so there are too many changes to list here, including many incompatibilities. This version may still be missing some functionality, so you may want to stick with 0.1.0 if you have problems. In particular, the new 'Chart' control is functional but 'Plot' is not. 'MPLPlot' has not been affected. I anticipate that only this pyqt version of pythics will be maintained in the future, and hopefully this version will be a solid base to build on. I do not expect many more major incompatibilities on this scale in future versions.

  * There is finally some useful documentation included with pythics, built using Sphinx. There is still a lot a writing to do, but the autodoc-generated API is already quite useful. See README.txt for more information.