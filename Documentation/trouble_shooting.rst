Trouble Shooting
----------------

* Language of a script cannot be recognized.

`It is not possible to see some scripts in the scripts panel`:

StepTalk is trying to determine script language by the file extension. Each
language has a extension list of files which contain scripts in that language.
To prevend expensive searching for file extensions in languages, StepTalk uses
a dictionary with file-language mapping. That dictionary is stored in
~/Library/StepTalk/Configuration and is created first time you use some
StepTalk tool. If you install new language, that file has to be updated.
    
Solution: run in terminal::

    $ stupdate_languages


