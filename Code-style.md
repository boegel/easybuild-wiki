The code style we follow in the EasyBuild code repository is mainly dictated by [PEP008](http://www.python.org/dev/peps/pep-0008/).

One big exception to PEP 8 is our preference of longer line lengths. We’re well into the 21st Century, and we have high-resolution computer screens that can fit way more than 79 characters on a screen. Don’t limit lines of code to 79 characters if it means the code looks significantly uglier or is harder to read, limit is 120 characters.

Principal outtakes from PEP8
* Use four spaces for indentation.
* Use optional underscores, not camelCase, for variable, function and method names (i.e. poll.get_unique_voters(), not poll.getUniqueVoters).
* Use InitialCaps for class names.
* In docstrings, don't use “action words”.
* Indent items in a list at an extra 4 spaces. nested lists can be indented at the same indentation as the first item in the list if it is on the first line, closing brackets must match visual indentation.

[`pep8`](https://github.com/jcrocholl/pep8) might be a useful tool to enforce the code style more strictly.


### Notes

Style guides that go a step beyond PEP0008:
 * http://www.gramps-project.org/wiki/index.php?title=Programming_guidelines
 * http://code.google.com/p/volatility/wiki/StyleGuide

Automatic rewriting of Python code: http://pypi.python.org/pypi/PythonTidy/1.22

Check PEP8 compliance: https://github.com/jcrocholl/pep8