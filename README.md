See http://kivy-garden.github.io/garden.filebrowser/index.html

[![Coverage Status](https://coveralls.io/repos/github/kivy-garden/filebrowser/badge.svg?branch=master)](https://coveralls.io/github/kivy-garden/filebrowser?branch=master)
[![Github Build Status](https://github.com/kivy-garden/filebrowser/workflows/Garden%20flower/badge.svg)](https://github.com/kivy-garden/filebrowser/actions)

FileBrowser
===========

The ``FileBrowser`` widget is an advanced file browser. You use it
similarly to FileChooser usage.

It provides a shortcut bar with links to special and system directories.
When touching next to a shortcut in the links bar, it'll expand and show
all the directories within that directory. It also facilitates specifying
custom paths to be added to the shortcuts list.

It provides a icon and list view to choose files from. And it also accepts
filter and filename inputs.

To create a ``FileBrowser`` which prints the currently selected file as 
well as the current text in the filename field when 'Select' is pressed,
with a shortcut to the Documents directory added to the favorites bar:

.. code-block:: python

    from kivy.app import App
    from os.path import sep, expanduser, isdir, dirname
    import sys

    class TestApp(App):

        def build(self):
            if sys.platform == 'win':
                user_path = dirname(expanduser('~')) + sep + 'Documents'
            else:
                user_path = expanduser('~') + sep + 'Documents'
            browser = FileBrowser(select_string='Select',
                                  favorites=[(user_path, 'Documents')])
            browser.bind(
                        on_success=self._fbrowser_success,
                        on_canceled=self._fbrowser_canceled)
            return browser

        def _fbrowser_canceled(self, instance):
            print 'cancelled, Close self.'

        def _fbrowser_success(self, instance):
            print instance.selection

    TestApp().run()

Events
------

- ``on_canceled``
  Fired when the `Cancel` buttons `on_release` event is called.
- ``on_success``
  Fired when the `Select` buttons `on_release` event is called.



Install
---------

```sh
pip install kivy_garden.filebrowser
```

CI
--

Every push or pull request run the [GitHub Action](https://github.com/kivy-garden/flower/actions) CI.
It tests the code on various OS and also generates wheels that can be released on PyPI upon a
tag. Docs are also generated and uploaded to the repo as well as artifacts of the CI.

TODO
-------

* add your code

Contributing
--------------

Check out our [contribution guide](CONTRIBUTING.md) and feel free to improve the flower.

License
---------

This software is released under the terms of the MIT License.
Please see the [LICENSE.txt](LICENSE.txt) file.

How to release
===============

* update `__version__` in `kivy-garden/filebrowser/__init__.py` to the latest version.
* update `CHANGELOG.md` and commit the changes
* call `git tag -a x.y.z -m "Tagging version x.y.z"`
* call `python setup.py bdist_wheel --universal` and `python setup.py sdist`, which generates the wheel and sdist in the dist/* directory
* Make sure the dist directory contains the files to be uploaded to pypi and call `twine check dist/*`
* then call `twine upload dist/*` to upload to pypi.
* call `git push origin master --tags` to push the latest changes and the tags to github.
