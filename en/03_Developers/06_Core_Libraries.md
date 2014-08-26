Core Libraries
--------------

QuikcAppsCMS features a number of global convenience functions that may come in
handy. Most of this functions are used by QuickAppsCMS's core classes, but many
others make working with arrays or strings a little easier.


### Global Constants and Functions


#### snapshot()

Stores some bootstrap-handy information into a persistent file.

Information is stored in `SITE/tmp/snapshot.php` file, it contains
useful information such as installed languages, content types slugs, etc.

You can read this information using `Configure::read()` as follow:

```php
Configure::read('QuickApps.<option>');
```

Or using the `quickapps()` global function:

```php
quickapps('<option>');
```



#### normalizePath(string $path, string $ds = DIRECTORY_SEPARATOR)

Normalizes the given file system path, makes sure that all DIRECTORY_SEPARATOR
are the same, so you won't get a mix of "/" and "\" in your paths.

***Example:***

```php
normalizePath('/some/path\to/some\\thing\about.zip');
// output: /some/path/to/some/thing/about.zip
```

You can indicate which "directory separator" symbol to use using the second
argument:

```php
normalizePath('/some/path\to//some\thing\about.zip', '\');
// output:
\some\path\to\some\thing\about.zip
```

By defaults uses DIRECTORY_SEPARATOR as symbol.



#### quickapps(string $key = null)

Shortcut for reading QuickApps's snapshot configuration.

For example, `quickapps('variables');` maps to 
`Configure::read('QuickApps.variables');`. If this function is used with no
arguments, `quickapps()`, the entire snapshot will be returned.



#### option(string $name, mixed $default = false)

Shortcut for getting an option value from "options" DB table.

The second arguments, $default, is used as default value to return if no value
is found. If not value is found and not default values was given this function
will return `false`.

**Example:**

```php
option('site_slogan');
```



#### listeners()

Returns a list of all registered event listeners in the system.



#### pluginName(string $name)

Used to extract plugin names from composer's package names.

**Example:**

```php
pluginName('quickapps/my-super-plugin');
// returns: MySuperPlugin
```

Package names must follow the "author/app-name" pattern, there are two
"especial" composer's package names which are handled differently:

- `php`: Will return "__PHP__"
- `quickapps/cms`: Will return "__QUICKAPPS__"



#### array_move(array $list, integer $index, string $direction)

Moves up or down the given element by index from a list array of elements.

If item could not be moved, the original list will be returned. Valid values for
$direction are `up` or `down`.

**Example:**

```php
array_move(['a', 'b', 'c'], 1, 'up');
// returns: ['a', 'c', 'b']
```



#### php_eval(string $code, array $args = [])

Evaluate a string of PHP code.

This is a wrapper around PHP's eval(). It uses output buffering to capture both
returned and printed text. Unlike eval(), we require code to be surrounded by
<?php ?> tags; in other words, we evaluate the code as if it were a stand-alone
PHP file.

Using this wrapper also ensures that the PHP code which is evaluated can not
overwrite any variables in the calling code, unlike a regular eval() call.

**Usage:**

```php
echo php_eval('<?php return "Hello {$world}!"; ?>', ['world' => 'WORLD']);
// output: Hello WORLD
```



#### get_this_class_methods(string $class)

Return only the methods for the given object. It will strip out inherited methods.



#### str_replace_once(string $search, string $replace, string $subject)

Replace the first occurrence only.

**Example:**

```php
echo str_replace_once('A', 'a', 'AAABBBCCC');
// out: aAABBBCCC
```



#### str_replace_last(string $search, string $replace, string $subject)

Replace the last occurrence only.

**Example:**

```php
echo str_replace_once('A', 'a', 'AAABBBCCC');
// out: AAaBBBCCC
```



#### str_starts_with(string $haystack, string $needle)

Check if $haystack string starts with $needle string.

**Example:**

```php
str_starts_with('lorem ipsum', 'lo'); // true
str_starts_with('lorem ipsum', 'ipsum'); // false
```



#### str_ends_with(string $haystack, string $needle)

Check if $haystack string ends with $needle string.

**Example:**

```php
str_ends_with('lorem ipsum', 'm'); // true
str_ends_with('dolorem sit amet', 'at'); // false
```



### language(string $key = null)

Retrieves information for current language.

Useful when you need to read current language's code, direction, etc.
It will return all the information if no `$key` is given.


**Usage:**

```php
language('code');
// may return: en-us
```

```php
language();
// may return:
[
    'name' => 'English',
    'code' => 'en-us',
    'iso' => 'en',
    'country' => 'US',
    'direction' => 'ltr',
    'icon' => 'us.gif',
]
```

Accepted keys are:

- `name`: Language's name, e.g. `English`, `Spanish`, etc.
- `code`: Localized language's code, e.g. `en-us`, `es`, etc.
- `iso`: Language's ISO 639-1 code, e.g. `en`, `es`, `fr`, etc.
- `country`: Language's country code, e.g. `US`, `ES`, `FR`, etc.
- `direction`: Language writing direction, possible values are "ltr" or "rtl".
- `icon`: Flag icon (it may be empty) e.g. `es.gif`, `es.gif`,
   icons files are located in Locale plugin's `/webroot/img/flags/` directory,
   to render an icon using HtmlHelper you should do as follow:

```php
<?php echo $this->Html->image('Locale.flags/' . language('icon')); ?>
```



#### user()

Retrieves current user's information (logged in or not) as an entity object.

**Usage:**

```php
$user = user();
echo user()->name;
// prints "Anonymous" if not logged in
```