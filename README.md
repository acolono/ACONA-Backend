# ACONA Backend

This project provides an administration page for managing ACONA users, domnains and configuration.
It is based on Drupal and Thunder.org and adds specific funtionality on top.

## Installation

1. Install 

See Thunder documentation [install documentation](https://thunder.github.io/thunder-documentation/quick-install) on how to use Thunder project.

2. Enable the following modules
- ACONA Configuration Suite

3. Enable the following theme
- ACONA

## Features
This package installs Thunder with some more contrib modules and ACONA specific features.

Contrib modules:
- drupal/group
- drupal/password_field
- drupal/restui
- drupal/views_bulk_edit
- drupal/entity_reference_actions


ao/acona_configuration: This is the ACONA Configuration Suite that provides the following:
- Metric Contentype
- PageType Contenttype
- URL Contenttype
- Service Contenttype
- Variable Types Vocabulary
- Services Vocabulary
- Units Vocabulary
- Domain Group
- Variable Priority Paragraph

ao/acona is the Drupal theme that is based on Olivero.

## Usage

First you need to create a user. When done you can set up a domain (group) and configure it:

![Adding domain](https://github.com/acolono/ACONA-Backend/blob/main/documentation/acona-backend_adding-domain.png)

![Configure a domain](https://github.com/acolono/ACONA-Backend/blob/main/documentation/acona-backend_domain-settings.png)

1. Activate a Data Source (Service)
In the next step you need to activate the data source or service.
You can use data from your Matomo instance or Google Search Api (at the moment only Matomo is active, but it will be expanded shortly). 
Click on button "Create new Data Source" to add your Source.
For this you need to add a title and select service type in a dropdown.
For authentication you want to add an API Key / Auth token for e.g. Matomo and save. 

2. Create and set up your Page Types
Set up page types with its specific Success Scores. For example you can create a page called "landing page" for which a combination of Organic Clicks and a low Bounce Rate defines the Success Score.

You need to add a title and select a variable, variable type and relevance. 
Selecting a correct variable type is important to calculate a meaningful success value between 0 and 100. If it happens that one of your variables is once greater as defined here, its okay - it can just happen that your success value is greater than 100 then.
Select the variable relevance from 1-100. The sum of all variables should be 100. 
You can add up to three variables for every page type. Just click on the "Add variable priority" button at the bottom. 

![Configure a Page Type](https://github.com/acolono/ACONA-Backend/blob/main/documentation/acona-backend_create-pagetype.png)

3. Set up URLs to be analyzed
At the moment you can select up to 5 URLs that should be analyzed. This will be expanded in the future and it will be possible to also analyze the whole domain.
Write a title and add an exact full URL such as https://example.com. Select a page type, e.g. "landing page", "product page" etc. You can also save some internal notes.

# More infos for developers
## What does the template do?

When installing the given `composer.json` some tasks are taken care of:

* Drupal will be installed in the `web`-directory.
* Autoloader is implemented to use the generated composer autoloader in `vendor/autoload.php`,
  instead of the one provided by Drupal (`docroot/vendor/autoload.php`).
* Modules (packages of type `drupal-module`) will be placed in `web/modules/contrib/`
* Theme (packages of type `drupal-theme`) will be placed in `web/themes/contrib/`
* Profiles (packages of type `drupal-profile`) will be placed in `web/profiles/contrib/`
* Downloads Drupal scaffold files such as `index.php`, or `.htaccess`
* Creates `sites/default/files`-directory.
* Latest version of drush is installed locally for use at `bin/drush`.


### File update

This project will attempt to keep all of your Thunder and drupal core files up-to-date; the 
project [drupal-composer/drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) 
is used to ensure that your scaffold files are updated every time drupal/core is 
updated. If you customize any of the "scaffolding" files (commonly .htaccess), 
you may need to merge conflicts if any of your modfied files are updated in a 
new release of Drupal core.

Follow the steps below to update your thunder files.

1. Run `composer update`
1. Run `git diff` to determine if any of the scaffolding files have changed. 
   Review the files for any changes and restore any customizations to 
  `.htaccess` or `robots.txt`.
1. Commit everything all together in a single commit, so `docroot` will remain in
   sync with the `core` when checking out branches or running `git bisect`.
1. In the event that there are non-trivial conflicts in step 2, you may wish 
   to perform these steps on a branch, and use `git merge` to combine the 
   updated core files with your customized files. This facilitates the use 
   of a [three-way merge tool such as kdiff3](http://www.gitshah.com/2010/12/how-to-setup-kdiff-as-diff-tool-for-git.html). This setup is not necessary if your changes are simple; 
   keeping all of your modifications at the beginning or end of the file is a 
   good strategy to keep merges easy.

## FAQ

### Should I commit the contrib modules I download

Composer recommends **no**. They provide [argumentation against but also 
workrounds if a project decides to do it anyway](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull 
request is often a better solution), you can do so with the 
[composer-patches](https://github.com/cweagans/composer-patches) plugin.

To add a patch to drupal module foobar insert the patches section in the extra 
section of composer.json:
```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL or local path to patch"
        }
    }
}
```
### Should I commit the scaffolding files?

The [drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) plugin can download the scaffold files (like
index.php, update.php, …) to the web/ directory of your project. If you have not customized those files you could choose
to not check them into your version control system (e.g. git). If that is the case for your project it might be
convenient to automatically run the drupal-scaffold plugin after every install or update of your project. You can
achieve that by registering `@drupal-scaffold` as post-install and post-update command in your composer.json:

```json
"scripts": {
    "drupal-scaffold": "DrupalComposer\\DrupalScaffold\\Plugin::scaffold",
    "post-install-cmd": [
        "@drupal-scaffold",
        "..."
    ],
    "post-update-cmd": [
        "@drupal-scaffold",
        "..."
    ]
},
```
### How can I prevent downloading modules from Thunder, that I do not need?

To prevent downloading a module, that Thunder provides but that you do not need, add a replace block to your composer.json:

```json
"replace": {
    "drupal/features": "*"
}
```

This example prevents any version of the feature module to be downloaded.
