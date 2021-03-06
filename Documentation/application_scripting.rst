+++++++++++++++++++++
Application Scripting
+++++++++++++++++++++

Scripting for applications is provided by the Application Scripting Bundle. The
bundle is installed together with StepTalk.

Creating a scriptable application
=================================

1.   Think of objects you want to provide for scripting.
2.   Make classes available in ScriptingInfo.plist:

.. code-block:: objective-c

    {
        STClasses = 
        {
            MessageComposition,
            InternetAddress
        };
    }
    
    Add this line to your makefile:
    
        MyApp_RESOURCE_FILES = ScriptingInfo.plist


3. Include bundle loading code. Copy files::
        STScriptingSupport.h 
        STScriptingSupport.m 

    from ApplicationScripting/Support directory to your project and add 
    following line to your makefile::

        MyApp_OBJC_FILES += STScriptingSupport.m

4. Make scripting available to the user

.. code-block:: objective-c

    #import "STScriptingSupport.h"

    ...
        if([NSApp isScriptingSupported])
        {
            [menu addItemWithTitle: @"Scripting"
	                    action: NULL
                     keyEquivalent: @""];

            [menu setSubmenu: [NSApp scriptingMenu]
                     forItem: [menu itemWithTitle:@"Scripting"]];
        }
    ...

Scripts
=======

Application is looking for scripts in:

* applications resource directory `ApplicationName.app/Resources/Scripts`
* application specific scripts in all GNUstep Library directories
  `/Library/StepTalk/Scripts/ApplicationName`
* shared scripts in all GNUstep Library directories `/Library/StepTalk/Scripts/Shared`
* resource directories of all bundles loaded by the application `BundleName.bundle/Resources/Scripts`

(*) can be any of GNUstep System, Local, Network or user path


Script metafile
---------------

Each script may have accompaining file containing information about script.
This information file is optional, has extension ``.stinfo`` and its name is script
name with that extension. For example if script name is ``insertDate.st`` then
information file is ``insertDate.st.stinfo``. File may contain:

* script name that will be shown to the user (localizable)
* script description (localizable)
* scripting language used for script -- overrides language guess based on file
  extension

The file is dictionary property list. Kes are:

* ``Name`` - Name of a script that is shown to the user. It can be localized.
* ``Description`` – Description of a script. It can be localized.
* ``Language`` – Scripting language name used in script. This value overrides
  language guess based on script file extension.

Localizable keys have values that are dictionaries:

.. code-block:: plist

    {
        Default = { 
            Name = "Some name";
            Description = "Some description";
        };
        English = { 
            Name = "Some name in english";
            Description = "Some description in english";
        };
        French = { 
            Name = "Some name in french";
            Description = "Some description in french";
        }
    }

Example
-------

1. Create a script file test.st with contents:

.. code-block:: smalltalk

    Transcript showLine:'It works.'

2. Create (optional) meta file `test.st.stinfo` with contents:

.. code-block:: plist

    {
        English = 
        {
            Description = "This is a script for testing if scripting works";
            Name = "Test";
        }
    }

3. Put both files into */Library/StepTalk/Scripts/your_application_name`

4. Then run your application, open scripts panel, select the script and run it
   by doubleclicking or by pressing the 'Run' button.

5. Then look at the transcript window for the result.
