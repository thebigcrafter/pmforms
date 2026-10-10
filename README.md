# pmforms

`pmforms` is a Virion library for creating interactive forms in PocketMine-MP plugins. It keeps the `dktapps\pmforms` namespace and provides closure-based handlers for form submissions and closes.

This repository preserves the upstream-compatible package metadata and API. For an example plugin, see the [pmforms demo](https://github.com/dktapps-pm-pl/pmforms-demo). The [upstream project](https://github.com/dktapps-pm-pl/pmforms) is also useful when comparing API history.

## Compatibility

- PHP: `^8.1`
- PocketMine-MP API: `4.0.0` and `5.0.0`, as declared by `virion.yml`
- Virion specification: `3.0`
- Namespace: `dktapps\pmforms`

The server and its API are supplied by the plugin's PocketMine-MP runtime. This library is not a standalone plugin and should not be installed as one.

## Form types

The library provides these form classes:

- `MenuForm` displays a title, text, and selectable `MenuOption` buttons. Its submit handler receives the player and selected option index.
- `ModalForm` displays a yes/no choice. Its submit handler receives the player and a boolean choice.
- `CustomForm` combines named input elements. Its submit handler receives the player and a `CustomFormResponse` object.
- `ServerSettingsForm` extends `CustomForm` and can include a `FormIcon` for server settings screens.

Custom forms support `Dropdown`, `Input`, `Label`, `Slider`, `StepSlider`, and `Toggle` elements. Element values are validated before a `CustomFormResponse` is delivered to the submit handler. Responses can be read with `getInt()`, `getString()`, `getFloat()`, `getBool()`, or `getAll()`.

All form submit handlers may be accompanied by an optional close handler that receives the player.

## Example

```php
use dktapps\pmforms\MenuForm;
use dktapps\pmforms\MenuOption;
use pocketmine\player\Player;

$player->sendForm(new MenuForm(
	"Settings",
	"Choose an option:",
	[
		new MenuOption("Open settings"),
		new MenuOption("Close"),
	],
	function(Player $player, int $selectedOption): void{
		if($selectedOption === 0){
			// Open the plugin's settings screen.
		}
	}
));
```

The example assumes that `$player` is a `pocketmine\player\Player` instance and that the library has been included in the consuming plugin's build.

## Including the library

`pmforms` is intended to be bundled into a plugin through a Virion-compatible build service. Use the metadata in `virion.yml` and the consumer's existing build configuration to select the dependency source and version. Do not copy the library source into the plugin manually; the consumer must test the resulting PHAR on its supported PocketMine-MP versions.

The package metadata is:

```yaml
name: pmforms
version: 2.3.4
antigen: dktapps\pmforms
api: [4.0.0, 5.0.0]
```

## Development

Install the development dependencies and run the static analysis configured by the repository:

```sh
composer install
vendor/bin/phpstan analyse
```

The GitHub Actions workflow runs PHPStan compatibility analysis across the supported PocketMine-MP and PHP matrix. Runtime behavior, form rendering, and consumer PHAR integration must be verified by the plugin that bundles this library.

## License

`pmforms` is licensed under the [GNU Lesser General Public License v3.0](LICENSE).
