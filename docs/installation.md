# Installation

## Step-by-step instructions

1. Install the `django-tailwind` package via `pip`:

   ```bash
   python -m pip install django-tailwind
   ```

   Alternatively, you can install the latest development version via:

   ```bash
   python -m pip install git+https://github.com/timonweb/django-tailwind.git
   ```
   
2. Add `'tailwind'` to `INSTALLED_APPS` in `settings.py`:
   ```python
   INSTALLED_APPS = [
     # other Django apps
     'tailwind',
   ]
   ```

3. Create a *Tailwind CSS* compatible *Django* app, I like to call it `theme`:

   ```bash
   python manage.py tailwind init
   ```
   > During the initialization step, you'll be prompted to choose between **Just in time (`jit`)** and **Ahead of time (`aot`)** modes. Whereas `jit` mode is new and somewhat experimental in *Tailwind CSS*, I suggest choosing it for the best development experience.
   > You can change the mode later via a simple config update. Check [jit vs aot](./jit-vs-aot.md) for more info.

4. Add your newly created `'theme'` app to `INSTALLED_APPS` in `settings.py`:
   ```python
   INSTALLED_APPS = [
     # other Django apps
     'tailwind',
     'theme'
   ]
   ```

5. Register the generated `'theme'` app by adding the following string in the `settings.py`:

   ```python
   TAILWIND_APP_NAME = 'theme'
   ```

6. Run the following command to install all necessary dependencies of *Tailwind CSS*:

   ```bash
   python manage.py tailwind install
   ```

7. *Django Tailwind* comes with a simple `base.html` template located at
   `your_tailwind_app_name/templates/base.html`. You can always extend it or delete it if you already have a layout.

8. If you don't use `base.html` template provided with *Django Tailwind*, add `{% tailwind_css %}` to the `base.html` template file:

   ```html
   {% load tailwind_tags %}
   ...
   <head>
      ...
      {% tailwind_css %}
      ...
   </head>
   ```
   
   The `{% tailwind_css %}` tag loads proper stylesheets and, when you're in the `DEBUG` mode, attaches to the `browser-sync` service for hot reloading of assets and pages.

9. Ok, you should now be able to use *Tailwind CSS* classes in HTML.

## PurgeCSS setup

To avoid importing all Tailwind CSS classes (that will result in a massive CSS file size), set up the `purge` configuration in `tailwind.config.js`.
This file is located in the `static_src` folder of the app created by `python manage.py tailwind init {APP_NAME}`.

For example, your `theme/static_src/tailwind.config.js` file could look like:

```js
module.exports = {
  purge: [
    // Templates within theme app (e.g. base.html)
    '../templates/**/*.html',
    // Templates in other apps
    '../../templates/**/*.html',
  ],
  ...
}
```

Note that you may need to adjust those paths to suit your specific project layout. It is crucial to ensure that *all* of your HTML files (or files containing HTML content, such as `.vue` or `.jsx` files) are covered under the `purge` rule.

For more information on the `purge` setup, check out the *"Controlling File Size"* page of the Tailwind CSS docs: [https://tailwindcss.com/docs/controlling-file-size/#removing-unused-css](https://tailwindcss.com/docs/controlling-file-size/#removing-unused-css) - particularly the *"Removing Unused CSS"* section, although the entire page is a helpful reference.

*The following applies to the `default` mode only.* 
To help speed up development builds, PurgeCSS is only run when you use the `python manage.py tailwind build` management command (creates a production CSS build).

If you run *Tailwind CSS* in `jit` mode, you get an optimized build even in the development mode, and it happens at lightning speed.

## `npm` executable path configuration

*Tailwind CSS* requires you to have *Node.js* installed on your machine.
*Node.js* is a *JavaScript* runtime that allows running *JavaScript* code outside a browser. Most (if not all) of the current frontend tools depend on *Node.js*.

If you don't have *Node.js* installed on your machine, please follow installation instructions from [the official Node.js page](https://nodejs.org/).

Sometimes (especially on *Windows*), *Python* executable cannot find the `npm` binary installed in the system.
In this case, you need to set the `npm` executable path in *settings.py* file manually (*Linux/Mac*):

```python
NPM_BIN_PATH = '/usr/local/bin/npm'
```

On *Windows* it might look like:

```python
NPM_BIN_PATH = r"C:\Program Files\nodejs\npm.cmd"
```

Please note that the path for `npm` executable in your system may be different. To obtain the `npm` path in your system, try running the `which npm` command in a terminal.