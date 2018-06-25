# ABA File Generator

## Notes
The ABA format provided @ simonblee/aba-file-generator does not match specification as described at:
https://www.commbank.com.au/content/dam/robohelp/PDFS/commbiz_direct_credit_debit.pdf
https://www.cemtexaba.com/aba-format/cemtex-aba-file-format-details
https://www.nab.com.au/content/dam/nabconnectcontent/file-formats/NAB%20Connect%20Consolidated%20File%20Format%20Specification_V0.05.pdf


Descriptive record does not contain BSB or account #. These fields (2-18) are to be left blank

## Overview
Generates an aba file for bulk banking transactions with Australian banks.

## License
[MIT License](http://en.wikipedia.org/wiki/MIT_License)

## Installation
Copy the files where needed or install via composer:
```bash
composer require simonblee/aba-file-generator
```

## Usage
Create a generator object with the descriptive type information for this aba file:
```php
use AbaFileGenerator\Model\Transaction;
use AbaFileGenerator\Generator\AbaFileGenerator;

$generator = new AbaFileGenerator(
    '123-456', // bsb
    '12345678', // account number
    'CBA', // bank name
    'User Name',
    'Remitter',
    '175029', // direct entry id for CBA
    'Payroll' // description
);

// Set a custom processing date if required
$generator->setProcessingDate('tomorrow');
```

Create an object or array of objects implementing `AbaFileGenerator\Model\TransactionInterface`. A simple Transaction object
is provided with the library but may be too simple for your project:
```php
$transaction = new Transaction();
$transaction->setAccountName(...);
$transaction->setAccountNumber(...);
$transaction->setBsb(...);
$transaction->setTransactionCode(...);
$transaction->setReference(...);
$transaction->setAmount(...);
```

Generate the aba string and save into a file (or whatever else you want):
```php
$abaString = $generator->generate($transaction); // $transaction could also be an array here
file_put_contents('/my/aba/file.aba', $abaString);
```

## References
- http://www.anz.com/Documents/AU/corporate/clientfileformats.pdf
- http://www.cemtexaba.com/aba-format/cemtex-aba-file-format-details.html
- https://github.com/mjec/aba/blob/master/sample-with-comments.aba
