# FPDM - PDF Form Data Merge

A PHP library for filling PDF forms by merging data into form fields.

> **Fork Notice**: This is a **PHP 8.x compatible fork** of [codeshell/fpdm](https://github.com/codeshell/fpdm), originally based on [Olivier Plathey's FPDM script](http://www.fpdf.org/en/script/script93.php).

## Features

- Fill PDF text fields from PHP arrays or FDF files
- Checkbox support with automatic state detection
- Native form flattening (experimental) - make fields read-only without external tools
- PHP 8.0+ compatible
- No external dependencies (pure PHP)

## Installation

### Composer

```bash
composer require tmw/fpdm
```

### Manual

```php
require_once 'path/to/fpdm.php';
```

## Usage

### Basic Text Fields

```php
<?php
require 'vendor/autoload.php';

$fields = array(
    'name'    => 'John Doe',
    'address' => '123 Main St',
    'city'    => 'New York',
);

$pdf = new FPDM('template.pdf');
$pdf->Load($fields, true); // true = UTF-8, false = ISO-8859-1
$pdf->Merge();
$pdf->Output('F', 'filled.pdf'); // Save to file
```

### Checkboxes

Enable checkbox support to toggle checkboxes in your PDF forms:

```php
<?php
$fields = array(
    'agree_terms' => true,   // Checked
    'newsletter'  => false,  // Unchecked
);

$pdf = new FPDM('form.pdf');
$pdf->useCheckboxParser = true; // Enable checkbox support
$pdf->Load($fields, true);
$pdf->Merge();
$pdf->Output();
```

Checkbox state names (e.g., `Yes`/`Off`, `Oui`/`Off`) are automatically detected during parsing.

### Flatten (Experimental)

Make form fields read-only after filling to prevent users from editing:

```php
<?php
$pdf = new FPDM('form.pdf');
$pdf->Load(['name' => 'John Doe']);
$pdf->Flatten(); // Enable flatten mode
$pdf->Merge();
$pdf->Output('F', 'flattened.pdf');

// Or use shorthand:
$pdf->Merge(true); // true = flatten
```

> **⚠️ Experimental**: This feature sets the ReadOnly flag on all form fields. The form structure is preserved but fields cannot be edited. This is a native PHP implementation that doesn't require external tools like pdftk.

## Output Options

```php
$pdf->Output();              // Send to browser (inline)
$pdf->Output('D', 'f.pdf');  // Force download
$pdf->Output('F', 'f.pdf');  // Save to file
$pdf->Output('S');           // Return as string
```

## Debugging

Enable verbose mode to see parsing details:

```php
$pdf->set_modes('verbose', true);
$pdf->set_modes('verbose_level', 3); // 1-4, higher = more detail
```

> **Note**: Disable verbose mode before generating PDF output.

## Fork Changes

This fork includes:

- Full PHP 8.0 - 8.5 compatibility
- Enhanced checkbox parser with support for indirect `/AP` references
- Native form flattening (experimental) - sets ReadOnly flags without requiring pdftk
- Replaced deprecated `create_function()` with closures
- Fixed curly brace string access syntax
- Fixed `implode()` parameter order
- Added missing class property declarations

## Credits

- **Original Author**: [Olivier Plathey](http://www.fpdf.org/) (FPDM v2.9)
- **Upstream Fork**: [codeshell/fpdm](https://github.com/codeshell/fpdm)

## License

FPDF License
