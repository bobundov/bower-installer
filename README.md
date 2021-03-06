bower-installer
===============

Tool for installing bower dependencies that won't include entire repos. Although Bower works great
as a light-weight tool to quickly install browser dependencies, it currently does not provide much
functionality for installing specific "built" components for the client.

#Bower installs entire repositories

```javascript
{
  "name" : "test",
  "version": "0.1",
  "dependencies" : {
    "jquery-ui" : "latest"
  }
}
```
If `bower install` is run on this configuration file, the entire jquery-ui repository will be pulled down
and copied into a components directory. This repository is quite large, when probably only a built js and css
file are needed.  Bower conveniently provides the `bower list --paths` command to list the actual main files associated
with the components (if the component doesn't define a main, then the whole repository is listed instead).

#Bower Installer
Bower installer provides an easy way for the main files to be installed or moved to one or more locations. Simply add to 
your component.json an `install` key and `path` attribute: 

```javascript
{
  "name" : "test",
  "version": "0.1",
  "dependencies" : {
    "jquery-ui" : "latest"
  },
  "install" : {
    "path" : "some/path"
  }
}
```

Install bower-installer by executing

```bash
npm install -g bower-installer
```

From the terminal in the same directory as your component.json file, enter:
```bash
bower-installer
```

After executing this, `jquery.js` and `jquery-ui.js` will exist under `some/path` relative to the location of your
component.json file.

#Overriding main files
A lot of registered components for bower do not include component.json configuration. Therefore, bower does not know
about any "main files" and therefore, by default bower-installer doesn't know about them either. Bower-installer 
can override an existing main file path or provide a non-existant one:

```javascript
{
  "name" : "test",
  "version": "0.1",
  "dependencies" : {
    "jquery-ui" : "latest",
    "requirejs" : "latest"
  },
  "install" : {
    "path" : "some/path",
    "sources" : {
      "requirejs" : "components/requirejs/require.js"
    }
  }
}
```
If bower installer is run on this configuration, `require.js`, `jquery.js`, and `jquery-ui.js` will all appear under
`some/path` relative to your component.json file.

#Install multiple main files
For one reason or another you may want to install multiple files from a single component. You can do this by providing
an `Array` instead of a `String` inside the sources hash:

```javascript
{
  "name" : "test",
  "version": "0.1",
  "dependencies" : {
    "jquery-ui" : "latest"
  },
  "install" : {
    "path" : "some/path",
    "sources" : {
      "jquery-ui" : [
        "components/jquery-ui/ui/jquery-ui.js",
        "components/jquery-ui/themes/base/minified/jquery-ui.min.css"
      ]
    }
  }
}
```