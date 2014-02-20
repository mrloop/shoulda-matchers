# Documentation branch

This is the branch where the rubydocs for shoulda-matchers are published.

You really shouldn't have to do anything with this branch directly, as when you
make a new release, the docs are published automatically.

However, if you ever need to generate the docs or publish them manually:

* `rake docs:generate[VERSION]` will generate the docs for a particular version.
  This assumes that a branch for the version already exists. The
  shoulda-matchers repo will be cloned in a hidden folder (.shoulda-matchers),
  the branch for the version will be checked out, and then the docs will be
  generated in a folder called VERSION. When you push your changes, the new docs
  will be viewable at http://thoughtbot.github.io/shoulda-matchers/VERSION.

* `rake docs:publish[VERSION]` generates the docs for a particular version and
  then pushes those docs to GitHub.

* `rake docs:publish_latest` generates the docs for the latest version and then
  pushes those docs to GitHub.
