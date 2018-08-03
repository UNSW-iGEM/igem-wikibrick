# igem-wikibrick

A user-friendly tool that makes developing wikis and webpages for the iGEM wiki behave as much like normal web development as possible. Automates building, image, HTML, CSS, Javascript and Package uploads.

Warning: igem-wikibrick is still under fairly active development and may not behave as expected at all times. If you encounter an error, problem or issue, please report it via our [issue tracker](https://github.com/Virginia-iGEM/igem-wikibrick/issues). If you would like to contribute, feel free to fork and submit a pull request. See [contributing](#8-contributing) for more information.

## 1 Table of Contents

1. [**Table of Contents**](#1-table-of-contents)
2. [**Installation**](#2-installation)
3. [**Tutorial**](#3-tutorial)
4. [**Description**](#4-description)
5. [**FAQ**](#5-faq)
6. [**Todo**](#6-todo)
7. [**Dependencies**](#7-dependencies)
8. [**Attributions, Dependencies and Works Cited**](#8-attributions-and-works-cited)
9. [**Contribution**](#9-contribution)

## 2 Installation

For new projects, see our Yeoman generator, [generator-igemwiki](https://github.com/Virginia-iGEM/generator-igemwiki). **This option is strongly reccommended for users new to webdevelopment.** This will give you an entire template for your wiki in addition to installing the tool for you.

If you have an existing project, either an existing wiki you've been writing on igem.org, or content you have saved locally that you'd like to get on the iGEM server, and would like help moving to our tool, see the [migration guide](https://github.com/Virginia-iGEM/igem-wikibrick/tree/master/docs/migration-guide).

If you already have a project and already know what you're doing: `npm install -D igem-wikibrick`. Gulp commands and project structures can be found in the Programmer's Guide.

## 3 Tutorial

See the [tutorial](https://github.com/Virginia-iGEM/igem-wikibrick/tree/master/docs/tutorial) page. You *must* have already installed the tool, either with `generator-igemwiki`, or standalone by following the migration guide.

## 4 Programmer's Guide

See [programmer's guide](https://github.com/Virginia-iGEM/igem-wikibrick/tree/master/docs/programmers-guide). This is a more technical document containing information on the architecture of igem-wikibrick, and is valuable both for new users trying to get a deeper understanding of how the tool works and how to modify it, as well as for well-versed developers who have no need of an introduction, and are looking to modify the tool for their own use as quickly as possible.

## 5 Description

`igem-wikibrick` is a commandline build tool that automates uploading of content (HTML, CSS, JS, images and fonts) to the iGEM Wiki, provides preprocessing, postprocessing, templating and other build conveniences, and can run a local webserver hosting a copy of the in-development wiki, enabling faster iteration times than are possible with the builtin Edit functionality of the igem wiki, and making it easier for team members to use their favorite development tools, I.E. Sublime, Vim, BASH, and making it easier to version control wikis for more robust development and for use by future teams.

Our goal in building this tool was to enable us to create an iGEM website that would tell our team's story, and we'd like to share this tool with other teams who have the same goal.

### 5.1 Feature List

- **Command line-based, no-hassle build tool.** Write and test all of your HTML, CSS and JS on your own machine. Once installed, the command `gulp serve` runs a local webserver that will update when any changes are made to your project. Change your stylesheets or page layout, save your changes, and see the changes instantly. No messing around with a _save page_ button or working in a text editor without syntax highlighting or autocompletion.
- **URL Substitution, automated uploads.** What happens when you want to see your changes on your iGEM Wiki page? Once correctly configured, the command `gulp publish` will intelligently modify any relative URLs in your HTML files to be absolute, pointing to the correct locations on the iGEM wiki, before sending all your files to those locations using an API developed by Toronto's iGEM team, `igemwiki-api`. In under a minute, all of your content, images, stylesheets and scripts will be available, live, on your iGEM Wiki.
- **Flexible Build and Package support.** Becuase we're using `gulp`, a Node.js based task automation tool, you can add thousands of gulp plugins and Node.js modules to your wiki to do all the things you ever wished you could. Want to use more advanced templating to reuse as much code as you can? `npm install -D gulp-handlebars`. Want an interactive experience that teaches your users how your project works? `npm install -D impactjs`. Want fluid, scriptable animations that react to the user? `npm install -D createjs-tweenjs`. You have access to literally the largest repository of open source packages in existence, `npm`, with over 350,000 packages.
- **Markdown, Google Docs and Word Document Conversion [UNDER CONSTRUCTION].** Are you the only programmer on your team? Worse yet, are you _not a programmer at all,_ but you bit the bullet and volunteered to spearhead your team's wiki? Either way, `igem-wikibrick`'s got your back. (Future) integration of Markdown and word processor document compiling will make incorporating your teammates' work into the wiki seamless and easy.

## 5 FAQ

TODO

## 6 Todo

### 6.1 Known Issues

- `npm install` will not correctly install this package due to problems with npm's dependency resolution. Workaround is to run `npm update` directly following an `npm install`. The install script will automatically do this.
- `gulp publish -l` sometimes doesn't perform the URL replace. Reason is unknown; workaround is to enter `gulp build -l` then `gulp push:content -l` following a `gulp publish -l` that fails to rewrite relative URLs.
- Upload timeouts will _sometimes_ throw large, verbose errors that aren't caught by our error handling code.
  - Same for login timeouts
  - Fork igemwiki-api and modify line ~90 of upload.js to correctly catch timeout errors instead of erroring out before... Continuing execution on user input.
- Attempting to `gulp build -l` without first `gulp push:images -l` will result in a big error and broken URL substitutions. Tell user to `push:images` or `publish` before `live build`ing in error message instead of just vomiting exceptions.
- `gulp publish` can be performed without the `-l` flag, which makes no sense. An error should be thrown which tells the user that the `-l` flag is necessary for publishing.

### 6.2 High Priority

- Fix _igemwiki-override.scss_ so that it actually corrects for the wiki styles and the published site reflects the local build site 1:1
- Update tutorials to reflect Build-Tool's current state

### 6.3 Medium Priority

- Make the actual path/name of `imagemap.json` something defined in `config.js`, instead of hardcoded.
- Make it so that `gulp publish` automatically sets the environment to `live`, while `gulp serve`, at least in its current state, automatically sets the environment to development.
- Added error-checker that asks the user if they want to upload `dev` build files to the iGEM wiki, instead of just blindly uploading them. Should probably use a `lock` file of some kind under the `build` directory that indicates what the last build environment was.
- Modify `gulpfile.js` so that separate directories, `build-dev` and `build-live` are created for each respective build.
- Fix MathJax with [#10 - Misc from this article](https://2016.igem.org/Team:Peshawar/Wiki)
- Modify `gulp/unit/html.js` so that it also accepts markdown files.
  - The code's there, just commented out; need to keep markdown processor from mangling `<!DOCTYPE>` tag.
  - Should probably compile markdown files exclusively and load them in using templates.
  - Also add Google Drive support?
- Replace bower with npm development dependencies. Bower is being phased out, not just in our project but across the web.

### 6.4 Low Priority

- Wrap the whole tool in a custom commandline tool?
- Create shell scripts (`.sh`, `.bat` files) that automatically install Node.js and all required npm and bower dependencies for team members and future teams.
  - Add git hook that causes an npm install and bower install on package.json or bower.json change.
  - Must be added on a repository-by-repository basis
- Pick a JavaScript styleguide, fix the awful inconsistencies in style to adhere to it.
  - Standardize variable and function naming schemes.
  - Should probably 'use strict'.
- Replace bower with npm or yarn for managing live dependencies.

### 6.5 Questionable Value

- Eliminate synchronous file read in gulp task live/push.js
  - JSON.parse is synchronous anyways, so will need a different JSON library?
- Update build-tool so that it scans for existing files and whether or not their srcs have changed under `./build` before building. Would need some kind of hash of each file, or a look at the date-last-changed.
  - Probably not that necessary, just more work for little gain considering how fast builds are

## 7 Dependencies

### 7.1 Build Dependencies

See [packages.json](https://github.com/Virginia-iGEM/2018-wiki/blob/master/package.json)

### 7.2 Live Dependencies

See [bower.json](https://github.com/Virginia-iGEM/2018-wiki/blob/master/bower.json)

## 8 Attributions and Works Cited

This list is not up-to-date, and will be updated once the tool enters a stable state.

- University of Toronto: igemwiki-api, used to automate uploads to iGEM wiki
- iGEM Peshawar 2016: General information regarding wiki development

## 9 Contributing

If you're interested in JavaScript, Handlebars, Markdown, Webpack, templating libraries like Handlebars, view libraries like React.js or Angular, and the future of frameworks support the iGEM wiki, and would like to contribute to the tools used to make the `igemwiki` stack run, we welcome pull requests, issue submissiosn and emails at [virginia.igem@gmail.com](mailto:virginia.igem@gmail.com).

If you are on an iGEM team, helping us by submitting issues, or forking and pull requesting can count towards a collaboration for your team.

Links to repositories that make this tool work:

- The original [igemwiki-api](https://github.com/igemuoftATG/igemwiki-api), written by Toronto 2016's Julian Mazzitelli
  - Our fork of [igemwiki-api](https://github.com/Virginia-iGEM/igemwiki-api), which fixes a few bugs with Julian's code and may be pulled in the future. `igem-wikibrick` runs off of this library.
- [generator-igemwiki](https://github.com/Virginia-iGEM/generator-igemwiki), a Yeoman generator which depends on this tool.