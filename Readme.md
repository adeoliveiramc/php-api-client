# Mifiel PHP API Client

[![Build Status][travis-image]][travis-url]
[![Coverage Status][coveralls-image]][coveralls-url]

PHP SDK for [Mifiel](https://www.mifiel.com) API.
Please read our [documentation](http://docs.mifiel.com/) for instructions on how to start using the API.

## Installation

Just run `composer require mifiel/api-client` in the root your project or add `mifiel/api-client` to your [composer.json](https://getcomposer.org/).

```json
  "require": {
    "mifiel/api-client": "^1.0"
  }
```

And then execute `composer install`.

## Usage

To start using the API you will need an `APP_ID` and a `APP_SECRET` which will be provided upon request (contact us at hola@mifiel.com).

You will first need to create an account in mifiel.com since the `APP_ID` and `APP_SECRET` will be linked to your account.

Then you can configure the library with:

```php
  use Mifiel\ApiClient as Mifiel;
  Mifiel::setTokens('APP_ID', 'APP_SECRET');
  // if you want to use our sandbox environment use:
  Mifiel::url('https://sandbox.mifiel.com/api/v1/');
```

Document methods:

- Find:

  ```php
    use Mifiel\Document;
    $document = Document::find('id');
    $document->original_hash;
    $document->file;
    $document->file_signed;
    # ...
  ```

- Find all:

  ```php
    use Mifiel\Document;
    $documents = Document::all();
  ```

- Create:

> Use only **original_hash** if you dont want us to have the file.<br>
> Only **file** or **original_hash** must be provided.

  ```php
    use Mifiel\Document;
    $document = new Document([
      'file_path' => 'path/to/my-file.pdf',
      'signatories' => [
        [ 
          'name' => 'Signer 1', 
          'email' => 'signer1@email.com', 
          'tax_id' =>  'AAA010101AAA' 
        ],
        [ 
          'name' => 'Signer 2', 
          'email' => 'signer2@email.com', 
          'tax_id' =>  'AAA010102AAA'
        ]
      ]
    ]);
    // if you dont want us to have the PDF, you can just send us 
    // the original_hash and the name of the document. Both are required
    $document = new Document([
      'original_hash' => hash('sha256', file_get_contents('path/to/my-file.pdf')),
      'name' => 'my-file.pdf',
      'signatories' => ...
    ]);

    $document->save();
  ```

- Delete

  ```php
    use Mifiel\Document;
    Document::delete('id');
  ```

Certificate methods:

- Sat Certificates

  ```php
    use Mifiel\Certificate;
    $sat_certificates = Certificate::sat();
  ```

- Find:

  ```php
    use Mifiel\Certificate;
    $certificate = Certificate::find('id');
    $certificate->cer_hex;
    $certificate->type_of;
    # ...
  ```

- Find all:

  ```php
    use Mifiel\Certificate;
    $certificates = Certificate::all();
  ```

- Create
  
  ```php
    use Mifiel\Certificate;
    $certificate = new Certificate([
      'file' => 'path/to/my-certificate.cer'
    ])
    $certificate->save();
  ```

- Delete

  ```php
    use Mifiel\Certificate;
    Certificate::delete('id');
  ```

## Contributing

1. Fork it ( https://github.com/Mifiel/php-api-client/fork )
2. Create your feature branch (`git checkout -b feature/my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin feature/my-new-feature`)
5. Create a new Pull Request

[travis-image]: https://travis-ci.org/Mifiel/php-api-client.svg?branch=master
[travis-url]: https://travis-ci.org/Mifiel/php-api-client
[coveralls-image]: https://coveralls.io/repos/github/Mifiel/php-api-client/badge.svg?branch=master
[coveralls-url]: https://coveralls.io/github/Mifiel/php-api-client?branch=master
