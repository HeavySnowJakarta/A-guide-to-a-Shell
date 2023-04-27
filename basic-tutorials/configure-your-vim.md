# Configure your Vim

### Using Vim 8’s built-in package manager

Many users have got used to package managers like `vim-plug`. Unfortunately, `vim-plug` has many problems with a-Shell. It’s recommended to turn to Vim 8’s native package managers, instead. You can run `:help packages` inside Vim for more information.

#### Installing, updating and removing packages manually

For Vim 8 of a-Shell, packages can be stored at `~/Documents/.vim/pack/*/start` or `~/Documents/.vim/pack/*/opt`, where `*` means any name you like. Each package has its own dictionary, making it easier to upgrade or remove them. All packages located in `~/Documents/.vim/pack/*/start` will be loaded automatically when Vim starts, while all those at `~/Documents/.vim/pack/*/opt` won’t. Themes should be stored at the `opt` dictionaries to avoid unpredictable problems. For example, let’s try to install NERDTree:

```bash
# It’s supposed you’ve already created the dictionary.
$ cd ~/Documents/.vim/pack/mypackages/start/nerdtree
$ lg2 clone https://github.com/preservim/nerdtree.git
```

When you want to update the package, you just need to run `lg2 pull` at its dictionary, and when you want to remove it, just delete it’s dictionary completely would be okay.

#### Importing packages

For packages not to be loaded automatically, we just need to run a command inside Vim to load it:

```
:packadd <package>
```

You can also write the command to `.vimrc` to load it automatically and suitably for complex needs.

### Edit .vimrc file

`.vimrc` stores at `~/Documents` dictionary. Just add anything you want to it to configure your Vim!

### Manage packages automatically

{% hint style="info" %}
This part would be written when `pack` is released.
{% endhint %}

