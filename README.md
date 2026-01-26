# Agama Weblate Integration

This is just a helper repository for exchanging the data between the the
[Weblate](https://l10n.opensuse.org/projects/agama/) online tool and the main
[Agama]( https://github.com/openSUSE/agama) GitHub repository. It works like a
buffer to avoid too frequent updates, it allows to send multiple small changes
at once in a bigger batch.

If you want to contribute see the [CONTRIBUTING.md](./CONTRIBUTING.md) file,
for more details see the [Agama internationalization](
https://agama-project.github.io/docs/devel/i18n) documentation.


## Translation Statistics

### Web

[![Translation status](https://l10n.opensuse.org/widgets/agama/-/agama-web/multi-auto.svg)](https://l10n.opensuse.org/engage/agama/)

### Ruby Service

[![Translation status](https://l10n.opensuse.org/widgets/agama/-/agama-service-master/multi-auto.svg)](https://l10n.opensuse.org/engage/agama/)

### Rust

[![Translation status](https://l10n.opensuse.org/widgets/agama/-/agama-rust-master/multi-auto.svg)](https://l10n.opensuse.org/engage/agama/)


### Products

[![Translation status](https://l10n.opensuse.org/widgets/agama/-/agama-products-master/multi-auto.svg)](https://l10n.opensuse.org/engage/agama/)


## Resolving conflicts

When a conflict happens in Weblate during pushing the changes to GitHub then the
Weblate repository gets locked and does not accept any changes in the
translations. This should avoid creating even more conflicts or wasting
translators work for translating outdated messages.

To unlock the Weblate repository the conflicts need to be resolved.

### Manual resolution

You can merge the changes from the Weblate Git manually and resolve the
conflicts from command line.

```sh
# clone this repository
git clone git@github.com:agama-project/agama-weblate.git
cd agama-weblate

# add the Weblate remote for each Weblate translation component,
# see the note below about the credentials
git remote add weblate-agama-web "https://${USERNAME_APIKEY?}@l10n.opensuse.org/git/agama/agama-web"
git remote add weblate-agama-service "https://${USERNAME_APIKEY?}@l10n.opensuse.org/git/agama/agama-service-master"
git remote add weblate-agama-rust "https://${USERNAME_APIKEY?}@l10n.opensuse.org/git/agama/agama-rust-master"
git remote add weblate-agama-products "https://${USERNAME_APIKEY?}@l10n.opensuse.org/git/agama/agama-products-master"

# fetch the remotes
git remote update weblate-agama-web
git remote update weblate-agama-service
git remote update weblate-agama-rust
git remote update weblate-agama-products

# merge the remotes
git merge weblate-agama-web/master
git merge weblate-agama-service/master
git merge weblate-agama-rust/master
git merge weblate-agama-products/master

# now resolve the conflicts
git status
vim ...
git add ...
git commit

# after pushing the changes Weblate should automatically detect the resolved
# conflicts and it should unlock the locked translation project
git push
```

Your user name and the API key for the Weblate Git remote can be found at the
Weblate account settings page at
https://l10n.opensuse.org/accounts/profile/#api.

At the bottom of that page there is a sample "Accessing Git repositories" command like

> git clone 'https://YourUserName:D7yd...jON@l10n.opensuse.org/git/PROJECT/COMPONENT/'

Assign `USERNAME_APIKEY=YourUserName:D7yd...jON` or paste that line to the folowing command:
```sh
USERNAME_APIKEY=$(sed 's:.*https.//\([^@]*\)@.*:\1:')
```

### Resetting Weblate

Alternatively you can reset the Weblate changes and re-read the GitHub
repository from scratch. But because this might cause losing some translations
it should be used with caution.

You need the admin permissions in the Weblate repository for this. Go to the
respective translation component and use the `Manage` ->
`Repository maintenance` menu option. Then press the `Reset` button in the
danger zone section.
