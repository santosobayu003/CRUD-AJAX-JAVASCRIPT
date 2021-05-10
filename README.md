# CRUD-AJAX-JAVASCRIPT
Cread, Read, Update, Delete menggunakan AJAX dan Framework CodeIgniter 4
1. Download framework CodeIgniter 4 di https://codeigniter.com/download atau melalui composer "composer create-project codeigniter4/appstarter CRUD-AJAX"

## model
```php
<?php
namespace App\Models;

use CodeIgniter\Model;

class M_biodata extends Model
{
    protected $table      = 'biodata';
    protected $primaryKey = 'id_biodata';

    protected $allowedFields = ['name', 'address', 'created_at'];
}
```
