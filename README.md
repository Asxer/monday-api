# Monday API
Monday.com API

## Installation & loading
Monday API is available on [Packagist](https://packagist.org/packages/tblack-it/monday-api) (using semantic versioning), and installation via [Composer](https://getcomposer.org) is the recommended way to install Monday API. Just add this line to your `composer.json` file:

```json
"tblack-it/monday-api": "~0.1"
```

or run

```sh
composer require tblack-it/monday-api
```

Note that the `vendor` folder and the `vendor/autoload.php` script are generated by Composer; they are not part of Monday API

Examples
--------

Init Monday connector
```php
<?php

require 'vendor/autoload.php';

$token = 'API_TOKEN';
$MondayBoard = new TBlack\MondayAPI\MondayBoard();
$MondayBoard->setToken(new TBlack\MondayAPI\Token($token));

```

Interact with boards
```php

# Get all boards
$all_boards = $MondayBoard->getBoards();

# Get Board id : 10012
$board_id = 10012;
$board = $MondayBoard->on($board_id)->getBoards();

# Get Board Columns
$board_id = 10012;
$boardColumns = $MondayBoard->on($board_id)->getColumns();

# Create Board, if success return board_id
$newboard = $MondayBoard->create( 'New Board Name', TBlack\MondayAPI\ObjectTypes\BoardKind::PUB );
$board_id = $newboard['create_board']['id'];

```

Interact with Itens
```php
# Insert new Item on Board
$board_id = 10012;
$id_group = 'topics';
$column_values = [ 'text1' => 'Value...','text2' => 'Other value...' ];

$addResult = $MondayBoard
              ->on($board_id)
              ->group($id_group)
              ->addItem( 'My Item Title', $column_values );

# if succes return
$item_id = $addResult['create_item']['id'];

# For update Item
$item_id = 34112;
$column_values = [ 'text1' => 'New Value','text2' => 'New other value...' ];

$updateResult = $MondayBoard
              ->on($board_id)
              ->group($id_group)
              ->changeMultipleColumnValues($item_id, $column_values );

# Archive item
$result = $MondayBoard
              ->on($board_id)
              ->group($id_group)
              ->archiveItem($item_id);

// Delete item
$result = $MondayBoard
              ->on($board_id)
              ->group($id_group)
              ->deleteItem($item_id);

```

If you need specific action, you can run a custom Query or Mutation
```php

// Run a custom query
$query = '
boards (ids: 12121212) {
  groups (ids: group_id) {
    items {
      id
      name
      column_values {
        id
        text
        title
      }
    }
  }
}';

# For Query
$items = $MondayBoard->customQuery( $query );

# For Mutation
$items = $MondayBoard->customMutation( $query );
```
