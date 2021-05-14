# CRUD-AJAX-JAVASCRIPT
Cread, Read, Update, Delete menggunakan AJAX dan Framework CodeIgniter 4
1. Download framework CodeIgniter 4 di https://codeigniter.com/download atau melalui composer "composer create-project codeigniter4/appstarter CRUD-AJAX"
2. Download jQuery 3.6.0 di https://jquery.com/download
3. Download bootstrap 4 di https://getbootstrap.com

### Model CodeIgniter 4
```php
<?php
namespace App\Models;

use CodeIgniter\Model;

class M_biodata extends Model
{
    protected $table      = 'tb_biodata';
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

### Add new data

```php
<div class="container">
    <div class="row">
        <div class="col-12 col-sm-8">
            <br>
            <h1 class="text-center">ADD BIODATA</h1> <br>

            <a class="btn btn-primary btn-sm" href="<?= base_url('C_biodata') ?>">All_Data</a> <br><br>

            <form method="post" name="form_insert_biodata">
                <div class="mb-3">
                    <label class="form-label">Your Name</label>
                    <input type="text" id="name" name="name" class="form-control form-control-sm validation" placeholder="Your Name">
                </div>
                <div class="mb-3">
                    <label class="form-label">Your Address</label>
                    <input type="text" id="address" name="address" class="form-control form-control-sm validation" placeholder="Your Address">
                </div>

                <br>
                <button type="button" id="save_biodata" class="btn btn-danger btn-sm" style="float: right;">Save</button>
            </form>

        </div>
    </div>
</div>

<script type="text/javascript">
    $(document).ready(function() {

        $('#save_biodata').on('click', function() {
            if (validation = "") {
                alert('Maaf data haarus diisi')
                return false;
            } else {
                alert('oke')
            }
            var name = $('#name').val();
            var address = $('#address').val();
            if (name == "") {
                alert("Nama harus terisi");
                return false;
            } else if (address == "") {
                alert("Alamat harus terisi");
                return false;
            } else {
                $.ajax({
                    url: "<?= base_url('C_biodata/insert_biodata') ?>",
                    type: "POST",
                    dataType: 'json',
                    data: {
                        name: name,
                        address: address,
                    },
                    success: function(data) {
                        console.log(data);
                        location.href = "<?= base_url('C_biodata') ?>";
                    }
                });
            }

        });
    });
</script>
```
