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
        return view('biodata/read_biodata');
    }

    public function read_biodata()
    {
        $data = $this->M_biodata->findAll();
        echo json_encode($data);
    }

    public function view_create_biodata()
    {
        return view('biodata/create_biodata');
    }

    public function view_update_biodata($id)
    {
        $data['data_id'] = $this->M_biodata->select('*')->where('id_biodata', $id)->get()->getRowArray();
        return view('biodata/update_biodata', $data);
    }

    public function insert_biodata()
    {
        $add_name = $this->request->getPost('name');
        $add_address = $this->request->getPost('address');

        $add_Data = [
            'name' => $add_name,
            'address' => $add_address,
        ];
        $insert = $this->M_biodata->insert($add_Data);
        echo json_encode($add_Data);
    }

    public function update_biodata()
    {
        $id = $this->request->getPost('id');
        $add_name = $this->request->getPost('name');
        $add_address = $this->request->getPost('address');

        $add_Data = [
            'name' => $add_name,
            'address' => $add_address,
        ];
        $insert = $this->M_biodata->update($id, $add_Data);
        echo json_encode($add_Data);
    }

    public function delete_biodata()
    {
        $id = $this->request->getPost('id');
        $select_id = $this->M_biodata->where('id_biodata', $id)->delete();
        echo json_encode($select_id);
    }
}

```

### Create

```php
<?= $this->extend('template/ms_template.php') ?>
<?= $this->section('content') ?>

<div class="container">
    <div class="row">
        <div class="col-12 col-sm-8">
            <br>
            <h3 class="text-center">CREATE BIODATA</h3> <br>

            <a class="btn btn-primary btn-sm" href="<?= base_url('C_biodata') ?>">Cancel</a> <br><br>

            <form method="post">
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

<?= $this->endSection() ?>

```
### Read & Delete

```php
<?= $this->extend('template/ms_template.php') ?>
<?= $this->section('content') ?>

<div class="container">
    <div class="row">
        <div class="col-12">
            <br>
            <H3 class="text-center">DATA BIODATA</H3> <br>

            <a class="btn btn-primary btn-sm" href="<?= base_url('C_biodata/view_create_biodata') ?>">Create</a> <br>

            <table class="table">
                <thead>
                    <tr>
                        <td>No.</td>
                        <td>Name</td>
                        <td>Address</td>
                        <td class="text-center">Created_add</td>
                        <td class="text-center">Action</td>
                    </tr>
                </thead>
                <tbody id="show_data">
                </tbody>
            </table>

        </div>
    </div>
</div>

<script type="text/javascript">
    $(document).ready(function() {
        my_data();

        function my_data() {
            $.ajax({
                type: 'POST',
                url: '<?= base_url('C_biodata/read_biodata') ?>',
                dataType: 'json',
                success: function(data) {
                    var html = '';
                    var i;
                    for (i = 0, x = 1; i < data.length; i++, x++) {
                        html += '<tr>' +
                            '<td>(' + [x] + ')</td>' +
                            '<td>' + data[i].name + '</td>' +
                            '<td>' + data[i].address + '</td>' +
                            '<td class="text-center">' + data[i].created_at + '</td>' +
                            '<td class="text-center">' +
                            '<a href="<?= base_url('C_biodata/view_update_biodata') ?>/' + data[i].id_biodata + '" class="btn btn-success btn-sm ">Update</a>' + ' ' +
                            '<button type="button" class="btn btn-danger btn-sm" id="delete" data-id="' + data[i].id_biodata + '">Delete</button>' +
                            '</td>' +
                            '</tr>';
                    }
                    $('#show_data').html(html);
                    return false;
                }
            });
        }

        $('#show_data').on('click', '#delete', function() {
            var id = $(this).attr('data-id');
            $.ajax({
                url: "<?= base_url('C_biodata/delete_biodata') ?>",
                type: "POST",
                dataType: 'json',
                data: {
                    id: id,
                },
                success: function(data) {
                    console.log(data);
                    location.href = "<?= base_url('C_biodata') ?>";
                }
            });
        })

    })
</script>

<?= $this->endSection() ?>

```
## Update

```php
<?= $this->extend('template/ms_template.php') ?>
<?= $this->section('content') ?>

<div class="container">
    <div class="row">
        <div class="col-12 col-sm-8">
            <br>
            <h3 class="text-center">UPATE BIODATA</h3> <br>


            <a class="btn btn-primary btn-sm" href="<?= base_url('C_biodata') ?>">Cancel</a> <br><br>

            <form method="post">
                <div class="mb-3">
                    <label class="form-label">Id Biodata</label>
                    <input type="text" id="id" name="id" value="<?= $data_id['id_biodata']; ?>" class="form-control form-control-sm validation" placeholder="Your Name" readonly>
                </div>

                <div class="mb-3">
                    <label class="form-label">Your Name</label>
                    <input type="text" id="name" name="name" value="<?= $data_id['name']; ?>" class="form-control form-control-sm validation" placeholder="Your Name">
                </div>

                <div class="mb-3">
                    <label class="form-label">Your Address</label>
                    <input type="text" id="address" name="address" value="<?= $data_id['address']; ?>" class="form-control form-control-sm validation" placeholder="Your Address">
                </div>

                <br>
                <button type="button" id="edit_biodata" class="btn btn-danger btn-sm" style="float: right;">Update</button>
            </form>

        </div>
    </div>
</div>

<script type="text/javascript">
    $(document).ready(function() {

        $('#edit_biodata').on('click', function() {
            var id = $('#id').val();
            var name = $('#name').val();
            var address = $('#address').val();
            $.ajax({
                url: "<?= base_url('C_biodata/update_biodata') ?>",
                type: "POST",
                dataType: 'json',
                data: {
                    id: id,
                    name: name,
                    address: address,
                },
                success: function(data) {
                    console.log(data);
                    location.href = "<?= base_url('C_biodata') ?>";
                }
            });

        });

    });
</script>

<?= $this->endSection() ?>

```
