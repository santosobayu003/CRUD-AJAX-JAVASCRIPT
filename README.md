# CRUD-AJAX-JAVASCRIPT
Cread, Read, Update, Delete menggunakan AJAX dan Framework CodeIgniter 4
1. Download framework CodeIgniter 4 di https://codeigniter.com/download atau melalui composer "composer create-project codeigniter4/appstarter CRUD-AJAX"
2. Download jQuery 3.6.0 di https://jquery.com/download
3. Download bootstrap 4 di https://getbootstrap.com

## Model CodeIgniter 4
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

### Controller CodeIgniter 4
```php
<?php
namespace App\Controllers;

use CodeIgniter\Controller;

use App\Models\M_biodata;

class C_biodata extends Controller
{

    protected $M_biodata;

    function __construct()
    {
        $this->M_biodata = new M_biodata();
    }

    public function index()
    {
        return view('biodata/v_biodata');
    }

    public function read_biodata()
    {
        $data = $this->M_biodata->findAll();
        echo json_encode($data);
    }

    public function add_biodata()
    {
        return view('biodata/add_biodata');
    }
}

```
