```php
header('Content-Type: application/json; charset=utf-8');

$value = [
    'code' => 200,
    'msg' => 'OK',
    'data' => []
];

echo json_encode($value);
```
