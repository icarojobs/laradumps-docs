# Configuration

LaraDumps configuration is stored in `config/laradumps.php` file, and some options can be configured directly in your project's `.env` file.

You can also run `php artisan ds:init` at any time to modify the settings via the wizard.

## Configuration options

### Host

The host setting allows your project to communicate with LaraDumps Desktop app.

Modify the host to match project's environment. Change the `.env` file key `DS_APP_HOST`.

```bash
DS_APP_HOST=127.0.0.1
```

 You may configure a custom host value, if needed.

<style>
    td::before { display: none }
    <!-- @see https://github.com/docsifyjs/docsify/issues/794 -->
</style>

| **Environment**             |  **Host**            |
|-----------------------------|----------------------|
| Default                     | 127.0.0.1            |
| Docker/Sail                 | host.docker.internal |
| Homestead with VirtualBox   | 10.211.55.2          |
| Homestead with Parallels    | 10.211.55.2          |

> 📝 **Note**: If you are using `Docker` on a `Linux` host, you must perform additional configuration because. Automatic mapping is not available. Read more [here](https://github.com/laravel/sail/pull/222).

### Auto-Invoke

By default, LaraDumps desktop app will be invoked and gain focus whenever a new dump arrives.

To disable this behavior, change the `.env` file key `DS_AUTO_INVOKE_APP` to `false`.

You can also send [Quiet dumps](laravel/debug/usage?id=quiet-dump) and the app will not be invoked.

```bash
DS_AUTO_INVOKE_APP=true #enabled
```

---

### SQL Queries

You must have this feature enabled to capture [SQL Queries](laravel/debug/usage?id=sql-queries) and send them to the Desktop App.

To enable this feature, change the `.env` file key `DS_SEND_QUERIES` to `true`.

```bash
DS_SEND_QUERIES=true #enabled
```

---

### Laravel Logs

You must have this feature enabled to send [Laravel Logs](laravel/debug/usage?id=laravel-logs).

To enable this feature, change the `.env` file key `DS_SEND_LOGS` to `true`.

 ```bash
DS_SEND_LOGS=true #enabled
```

---

### Livewire Components

Tracking Livewire Components can be enabled and disabled in your project's .env file.

```bash
DS_SEND_LIVEWIRE_COMPONENTS=true #enabled
```

---

### Specify Livewire Components

You can specify which Livewire Components will be tracked by the Desktop application in your project's .env file.

Only components in this list will send state to the Desktop app.

```php
DS_LIVEWIRE_COMPONENTS="MyComponent,NotesComponent,AttachmentsComponent"
```

---

### Ignore Livewire Components

To ignore specific Livewire components, include each class path in the `ignore_livewire_components` key inside the config file.

Components in this list will NOT send state to the Desktop app.

```php
'ignore_livewire_components' => [
     \App\Http\Livewire\MyComponent::class,
     \App\Http\Livewire\NotesComponent::class,
],
```

---

### Livewire Validation

To enable dumping [Livewire failed validations](laravel/debug/usage?id=livewire-validation), change the key `DS_SEND_LIVEWIRE_FAILED_VALIDATION` to `true` inside your project's `.env` file.

You can also configure the interval (sleep time).

```bash
DS_SEND_LIVEWIRE_FAILED_VALIDATION=true #enabled
DS_SEND_LIVEWIRE_FAILED_VALIDATION_SLEEP=400 #milliseconds
```

---

### Livewire Auto-Clear

When debugging Livewire, you need to clear your LaraDumps APP history every time the page is reloaded to keep track of your components.

LaraDumps can automatically clear the screen on page reload. You must include the Auto-Clean Blade directive together with the Livewire directive.

```html
  <!-- Scripts -->
    @livewireScripts
    @if(app()->environment('local'))
        @dsAutoClearOnPageReload
    @endif
</body>
```

This feature is disabled by default. To enable it, change the configuration key `auto_clear_on_page_reload` to `true`.

```php
'auto_clear_on_page_reload' = true, //enabled
```

---

### Routes

You might want to skip some routes when dumping [Routes](laravel/debug/usage?id=routes).

Append the routes to be ignored under the `ignore_route_contains` configuration key.

By default, these routes are ignored:

```php
'ignore_route_contains' => [
    'debugbar',
    'ignition',
    'horizon',
    'livewire',
    'my-secret-route', // this route will be skipped
],
```

---

### Preferred IDE

Each dump contains a link to the file and line where the `ds()` was called from.

You may configure your preferred IDE to open this project's files.

To set an IDE, change the `.env` file key `DS_PREFERRED_IDE` to one of the supported IDEs.

```bash
DS_PREFERRED_IDE=vscode
```

Supported IDEs:

<style>
    td::before { display: none }
    <!-- @see https://github.com/docsifyjs/docsify/issues/794 -->
</style>

| **IDE**             |  **Value**  |
|---------------------|-------------|
| PHPStorm            | phpstorm    |
| Visual Studio Code  | vscode      |
| Remote Visual Studio Code (WSL2) | vscode_remote      |
| Sublime             | sublime     |
| Atom                | atom        |

You might also edit or add a new IDE handler. The IDEs are defined inside the configuration key `ide_handlers` in the configuration file.

#### Remote VSCode (WSL2)

Additional configuration is required for the  `Visual Studio Code WSL2`.

1․ You must set the preferred Linux distro in your project's .env file (by default: `Ubuntu20.04LTS`):

```bash
DS_PREFERRED_WSL_DISTRO=Ubuntu20.04LTS
```

2․ You must set the `remote_path` in your project's .env file:

```bash
DS_REMOTE_PATH=/home/$USER/path-to-your-project
```
PS: to get this path just type the following command on your terminal in wsl project folder:
```bash
$ pwd
```

3․ You must set the `work_dir` in your project's .env file (by default: `/var/www/html`):

```bash
DS_WORKDIR=/var/www/html
```

4. Here's what your `vscode_remote` key in the configuration file will look like:

```php
'vscode_remote' => [
    'handler'        => 'vscode://vscode-remote/',
    'line_separator' => ':',
    'local_path'     => 'wsl+' . env('DS_PREFERRED_WSL_DISTRO', 'Ubuntu20.04LTS'),
    'remote_path'    => env('DS_REMOTE_PATH', null),
    'work_dir'       => env('DS_WORKDIR', '/var/www/html'),
],
```

---

## DS Check

By default, the artisan [ds:check](laravel/debug/deploying-to-production) command is checking the config directory, which will always produce an error.

You must specify which directories you would like to be checked. This configuration is set in the key `ci_check.directories`.

```php
'ci_check' => [
    'directories' => [
        base_path('app'), //checks the app/ directory
    ],
    //...
],
```

---
