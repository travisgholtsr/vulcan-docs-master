## Working with release.sh

Many of our plugins use a shell script, `release.sh`, to handle pushing new versions to `wordpress.org`'s plugins SVN repository. The canonical copy of this file [is in this repository](./release.sh).

This file provides setup, usage, and configuration instructions for `release.sh` in your WordPress plugins.

### The first release

1. Review the files in the `BLACKLIST` variable in `release.sh`. Make changes as necessary.
2. Update the `SVN_REPO` variable with your plugin's `plugins.svn.wordpress.org` SVN repository.
3. `chmod +x release.sh`
4. Add `release/` to the project's `.gitignore`
5. Commit all changes and push your commits to the plugin's repository.

6. Run `release.sh`.
  - You may need to verify the receiving server's key fingerprint.
  - If it asks you to enter the password for your computer's username, press the `[enter]` key to get the username prompt, then enter your `wordpress.org` username and password.

### Later releases

1. Run `release.sh`

### Generating zip files for GitHub

GitHub's default release `.zip` files do not contain files installed by package management systems through commands like `npm install`, `bower install`, and `composer install`.
`release/wp-release.zip` is generated by `release.sh` for every release on the master branch, and contains everyhting that is put in the wordpress.org plugin repository.

If you don't have a recent `wp-release.zip`, generate one in the following manner:

1. Run `release.sh --dry_run`
2. When it asks you if you really want to deploy, type `y`: it will not actually deploy, because of the `--dry_run` flag.
3. Check for the existence of `release/wp-release.zip`.

Once you have `release/wp-release.zip`, upload `release/wp-release.zip` to GitHub as [a binary asset for the release](https://github.com/blog/1547-release-your-software). This can only be done for [tagged releases](https://help.github.com/articles/creating-releases/).

### Updating the `assets/` directory of `svn/`

`release.sh` only affects the `tags/` and `trunk/` directories. To update the assets contained in the `assets/` branch, such as [plugin assets](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/) for the WordPress.org listing, perform the following steps:

1. Add files to `svn/assets/` by whatever means you want
2. From `svn/`, run `svn add assets/*`
3. Check that the files are marked for addition with `svn status`
4. svn commit -m "Updating assets"
  - You may need to verify the receiving server's key fingerprint.
  - If it asks you to enter the password for your computer's username, press the `[enter]` key to get the username prompt, then enter your `wordpress.org` username and password.

WordPress.org's [How to use Subversion](https://developer.wordpress.org/plugins/wordpress-org/how-to-use-subversion/) article may be of interest, but it's focused on releasing releases and not updating assets.

### Plugins using `release.sh`

This list may be incomplete.

- The original script: https://github.com/publicmediaplatform/pmp-wordpress/blob/master/release.sh
- The current script: https://gist.github.com/rnagle/40d84cbd5fef86e3de7781fc31b46d94
- https://github.com/INN/analytic-bridge
- https://github.com/INN/DoubleClick-for-WordPress
- https://github.com/INN/link-roundups
- https://github.com/INN/super-cool-ad-inserter-plugin

Significant changes to `release.sh` should be copied between plugins.