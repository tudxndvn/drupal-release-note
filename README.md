# drupal-deploy-note
The document for deploy drupal using composer

# I. Backup database and code
- Export your database 
- Rename folder source code (E.g : iec-backup)
# II. Deploy new code
- Unzip `iec-composer-master.zip`
- `cd` to Source code directories and run `composer install`
- Setting database:
  open file setting.php (web/sites/default) and change config database . E.g:
    ```$databases['default']['default'] = array (
      'database' => 'iec_composer',
      'username' => 'root',
      'password' => 'root',
      'prefix' => '',
      'host' => 'localhost',
      'unix_socket' => '/Applications/MAMP/tmp/mysql/mysql.sock',
      'port' => '3306',
      'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
      'driver' => 'mysql',
  );
  $settings['install_profile'] = 'standard';
  $config_directories['sync'] = 'config/sync';
  $settings['class_loader_auto_detect'] = TRUE;```
- Update files upload : Replace folder `files` in sites/default/ from old source to web/core/sites/ (new source)
- Please note when config apache :  set DocumentRoot to folder `web`
- Run Website
# III. Note
## Updating Drupal Core
This project will attempt to keep all of your Drupal Core files up-to-date; the 
project [drupal-composer/drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) 
is used to ensure that your scaffold files are updated every time drupal/core is 
updated. If you customize any of the "scaffolding" files (commonly .htaccess), 
you may need to merge conflicts if any of your modified files are updated in a 
new release of Drupal core.

Follow the steps below to update your core files.

1. Run `composer update drupal/core webflo/drupal-core-require-dev "symfony/*" --with-dependencies` to update Drupal Core and its dependencies.
1. Run `git diff` to determine if any of the scaffolding files have changed. 
   Review the files for any changes and restore any customizations to 
  `.htaccess` or `robots.txt`.
1. Commit everything all together in a single commit, so `web` will remain in
   sync with the `core` when checking out branches or running `git bisect`.
1. In the event that there are non-trivial conflicts in step 2, you may wish 
   to perform these steps on a branch, and use `git merge` to combine the 
   updated core files with your customized files. This facilitates the use 
   of a [three-way merge tool such as kdiff3](http://www.gitshah.com/2010/12/how-to-setup-kdiff-as-diff-tool-for-git.html). This setup is not necessary if your changes are simple; 
   keeping all of your modifications at the beginning or end of the file is a 
   good strategy to keep merges easy.
   
## How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull request is often a better solution), you can do so with the composer-patches plugin.

To add a patch to drupal module foobar insert the patches section in the extra section of composer.json:
```
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL or local path to patch"
        }
    }
}
```

> Link refer : https://github.com/drupal-composer/drupal-project
