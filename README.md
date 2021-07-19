# Hugo Minimal Test Theme

This theme is not intended to be used as a normal theme. It is intended as a framework for creating minimal reproducible test cases. As such, one does not simply drop it in a site, but rather creates a repository specifically designed to isolate and demonstrate the issue one is facing.

The goal is to minimize the amount of 'layout grunt work' one has to do to create the test case.

It includes the ability to enable less minimal features via custom layouts (with thanks to members of the [Hugo Forum](https://discourse.gohugo.io) for their suggestions).

See [License](https://github.com/danielfdickinson/hugo-minimal-test/blob/master/LICENSE) for permissions on use.

## Modern Hugo Only (as of 2021-03-17)

This minimal theme is built using assumptions that may require Hugo 0.81.0 or higher, and in any event those are the only versions on which it has been tested, at present.

## Available Features

### A Note on Layout Feature Selection

As @davidsneighbour on the [Hugo Forum](https://discourse.gohugo.io) [suggested in response to my announcement of this theme](https://discourse.gohugo.io/t/a-theme-for-minimal-reproducible-test-casing/31790/6), features that are less 'minimal', but still needed in order to have a minimal reproducible test case for more complex (even if barely) situations than an absolute minimal theme, can be enabled through the use of custom layouts.

If a feature is listed in the table below, one can activate the feature on a page by adding ``layouts = "feature-layout"`` (for toml frontmatter) or ``layouts: feature-layout`` (for yaml frontmatter) to the page metadata in the page file in the ``contents`` directory. (One can also set per-page metadata in ``config.toml`` but for that see the [Hugo Documentation](https://gohugo.io/documentation/)).

Not that features are often specific to the type of page (e.g. list pages vs. single pages) and when that is the case it is noted under 'Applies to'.

This is possible through the magic of [Hugo's Lookup Order](https://gohugo.io/templates/lookup-order).

Obviously for ``feature-layout`` use the layout from the table below.

#### Layout Feature Table

| Feature                        | feature-layout        | Applies to              | Description |
|--------------------------------|-----------------------|-------------------------|-------------|
| Custom Layout Testing          | custom                | sections and single     | A generic custom layout (just announces it is a custom layout with a heading) that will work with both a section and a single page.          |
| Layout Order Testing           | layout-order          | sections and single     | A section page should display the section ``layout-order`` layout not the single page``layout-order`` layout. A single page should display the single page ``layout-order`` layout (the title will indicate which it is) |
| Layout Section Order Testing   | layout-section-order   | sections and single     | A section page should display a section ``layout-section-order`` layout not the single  page``layout-section-order`` layout for both typical sections as well as for pages in the special ``custom-section-layout-test`` section (directory). A single page should display the single page ``layout-section-order`` layout for both typical sections and a page in the special ``custom-section-layout-test`` section (directory). Once again the title will indicate which layout is in use. |
| Pagination                     | pagination            | section/list pages only | Displays a section as a list of the pages (both regular pages and section pages) in a section, dividing the list of pages in a section into separate sub-pages, each containing at most the number of pages from the section that is defined in the config's ``pagination`` setting (default 10). |

### Custom Sections

* Currently only ``custom-section-layout-test`` has special layouts one can choose to use with the section pages, or pages in the section (see "Layout Section Order Testing" above), although the section also has 'standalone' layout (i.e. without layout ``layout-section-order`` required).

### Custom Taxonomy

* Currently only ``taxonomy-test-taxonomy``
* To use it:
  * In ``config.toml`` have a section such as the following (with the appropriate taxonomies for your site):
    ```
    [taxonomies]
      category = "categories"
      tag = "tags"
      taxonomy-test-taxonomy = "taxonomy-test-taxonomies"
    ```
  * In page frontmatter that should be using the taxonomy (assuming toml frontmatter):
    ```
    taxonomy-test-taxonomies = ["term1","term2"]
    ```

### hello-world Shortcode

A simple shortcode that emits an HTML paragraph with 'Hello World!'. To use it,
add ``{{\< hello-world>}}`` to a **content** file (not template).
## Usage

1. Create a test case site, or clone a site with no additional layouts, and switch to it
   ```
   git clone https://github.com/danielfdickinson/hugo-test-minimal-theme
   cd hugo-test-minimal-theme
   ```
### Using Hugo Modules (preferred)

1. Initialize the Hugo module system: ``hugo mod init github.com/<your_user>/<your_project>`` (assuming you are using github, of course).
2. Import the theme in your ``config.html``
   ```
   [module]
     [[module.imports]]
        path = "github.com/danielfdickinson/hugo-minimal-test"
   ```
3. Change back to the site directory
4. Get the module
   ```
   hugo mod get github.com/danielfdickinson/hugo-minimal-test
   ```
5. To test the result, run the local Hugo server
   ```
   hugo server -b http://localhost:1313/
   ```
### Using downloaded copy of the theme (e.g. Zip from the Git repo)

1. Make a themes directory and switch to it
   ```
   mkdir themes
   cd themes
   ```

2. Obtain a copy of the theme e.g. ([a theme Zip file from the Git repo](https://github.com/danielfdickinson/hugo-minimal-test/archive/refs/head/main.zip))
3. Copy/extract the theme into hugo-minimal-test in the themes directory
4. Change back to the site directory
5. To test the result, run the local Hugo server
   ```
   hugo server -t hugo-minimal-test -b http://localhost:1313/
   ```
### Using git submodules (deprecated)

1. Make a themes directory and switch to it.
   ```
   mkdir themes
   cd themes
   ```

2. In the themes directory, add hugo-minimal-test as a submodule
   ```
   git submodule add -f https://github.com/danielfdickinson/hugo-minimal-test
   ```
3. Change back to the site directory
4. To test the result, run the local Hugo server
   ```
   hugo server -t hugo-minimal-test -b http://localhost:1313/
   ```

 Enjoy!

## Change Debug Table from Development Only

See full [hugoDebugTable README.md](https://github.com/danielfdickinson/hugo-debug-tables/blob/main/README.md)
for information on using the debug table included in this theme.

### To add to production builds

In ``config.toml`` add

```toml
[params]
     debugTableEnvironment = ["development","production"]
```

### To never include in builds

```toml
[params]
     debugTableEnvironment = [""]
```

**NB** The ``""`` is required because an empty slice (or no param) defaults to ``["development"]``

### You can also use custom environments

In ``config.toml`` add

```toml
[params]
     debugTableEnvironment = ["custom1","custom2"]
```

And of course if you wanted you could add ``"development"`` and/or ``"production"``

## Params for parts of the layout
To disable some layout in the table below:

In ``config.toml`` add

```toml
[params]
     <param listed below> = false
```

| Param                    | Description                                    |
|--------------------------|------------------------------------------------|
| showTopBar               | If false, do not show top menu bar             |
| testStylesInclude        | If true adds old browser warning because the offending styles are also enabled (see below). |

## Test CSS Styling

This makes the debug tables more usable and comes from the hugo-debug-tables repository.

To enable the styles:

In ``config.toml`` add

```toml
[params]
     testStylesInclude = true
```
### For the Debug Tables

See [hugo-debug-tables README.md](https://github.com/danielfdickinson/hugo-debug-tables/blog/main/README.md)

### For the theme itself

| Class                       | Description                                 |
|-----------------------------|---------------------------------------------|
| test-warningold-min         | Indicates warning about old browser         |
