# lab10web
# Danur Wenda Prasetiyo
# 312410008
# TI.24.A1

## 1. Tujuan Praktikum

Praktikum ini bertujuan agar mahasiswa:

Memahami konsep dasar Object-Oriented Programming (OOP).

Mengerti konsep class dan object pada PHP.

Mampu membuat program OOP sederhana menggunakan PHP.

Menerapkan modularisasi menggunakan class library seperti Form dan Database.

## 2. Langkah Kerja Praktikum
Persiapan Project

Buat folder baru pada webserver (XAMPP/htdocs):

```bash
htdocs/lab10_php_oop/
```

![ss](index.png)

## 3. Praktikum OOP – Program Mobil
## File: mobil.php

Program ini membuat class Mobil dengan property dan method, kemudian membuat dua objek berbeda.

Kode Program
```php
<?php
/**
* Program sederhana pendefinisian class dan pemanggilan class.
**/
class Mobil
{
    private $warna;
    private $merk;
    private $harga;

    public function __construct()
    {
        $this->warna = "Biru";
        $this->merk = "BMW";
        $this->harga = "10000000";
    }

    public function gantiWarna($warnaBaru)
    {
        $this->warna = $warnaBaru;
    }

    public function tampilWarna()
    {
        echo "Warna mobilnya : " . $this->warna;
    }
}

// membuat objek mobil
$a = new Mobil();
$b = new Mobil();

// memanggil objek
echo "<b>Mobil pertama</b><br>";
$a->tampilWarna();
echo "<br>Mobil pertama ganti warna<br>";
$a->gantiWarna("Merah");
$a->tampilWarna();

// memanggil objek
echo "<br><b>Mobil kedua</b><br>";
$b->gantiWarna("Hijau");
$b->tampilWarna();
?>
```

![ss](mobil.png)

## 4. Class Library — Form
## File: form.php

Class library ini digunakan untuk membuat form input secara dinamis.

Kode Program
```php
<?php
/**
 * Nama Class: Form
 * Deskripsi: Class untuk membuat form inputan text sederhana
 */

class Form
{
    private $fields = array();
    private $action;
    private $submit = "Submit Form";
    private $jumField = 0;

    public function __construct($action, $submit)
    {
        $this->action = $action;
        $this->submit = $submit;
    }

    public function displayForm()
    {
        echo "<form action='".$this->action."' method='POST'>";
        echo '<table width="100%" border="0">';

        for ($j = 0; $j < count($this->fields); $j++) {
            echo "<tr><td align='right'>".$this->fields[$j]['label']."</td>";
            echo "<td><input type='text' name='".$this->fields[$j]['name']."'></td></tr>";
        }

        echo "<tr><td colspan='2'>";
        echo "<input type='submit' value='".$this->submit."'></td></tr>";
        echo "</table>";
        echo "</form>";
    }

    public function addField($name, $label)
    {
        $this->fields[$this->jumField]['name'] = $name;
        $this->fields[$this->jumField]['label'] = $label;
        $this->jumField++;
    }
}
?>
```
## 5. Implementasi Form Library
## File: form_input.php

File ini meng-include form.php dan membuat form input sederhana.

Kode Program
```php
<?php
include "form.php";

echo "<html><head><title>Mahasiswa</title></head><body>";

$form = new Form("", "Input Form");
$form->addField("txtnim", "Nim");
$form->addField("txtnama", "Nama");
$form->addField("txtalamat", "Alamat");

echo "<h3>Silahkan isi form berikut ini :</h3>";
$form->displayForm();

echo "</body></html>";
?>
```
![ss](form_input.png)

## Class Library — Database Connection
## File: config.php

Konfigurasi database.

```php
<?php
$config = [
    'host' => 'localhost',
    'username' => 'root',
    'password' => '',
    'db_name' => 'latihan_oop'
];
```
## File: database.php

Class ini berfungsi untuk:

koneksi database

insert

update

delete

get data

Kode Program
```php
<?php

class Database {
    protected $host;
    protected $user;
    protected $password;
    protected $db_name;
    protected $conn;
    
    public function __construct() {
        $this->getConfig();
        $this->conn = new mysqli(
            $this->host, 
            $this->user, 
            $this->password, 
            $this->db_name
        );

        if ($this->conn->connect_error) {
            die("Connection failed: " . $this->conn->connect_error);
        }
    }
    
    private function getConfig() {
        include_once("config.php");
        $this->host = $config['host'];
        $this->user = $config['username'];
        $this->password = $config['password'];
        $this->db_name = $config['db_name'];
    }
    
    public function query($sql) {
        return $this->conn->query($sql);
    }
    
    public function get($table, $where = null) {
        if ($where) {
            $where = " WHERE " . $where;
        }

        $sql = "SELECT * FROM " . $table . $where;
        $sql = $this->conn->query($sql);
        return $sql->fetch_assoc();
    }
    
    public function insert($table, $data) {
        if (is_array($data)) {
            foreach($data as $key => $val) {
                $column[] = $key;
                $value[] = "'{$val}'";
            }

            $columns = implode(",", $column);
            $values = implode(",", $value);
        }

        $sql = "INSERT INTO ".$table." (".$columns.") VALUES (".$values.")";
        $result = $this->conn->query($sql);

        return $result ? true : false;
    }
    
    public function update($table, $data, $where) {
        $update_value = "";

        if (is_array($data)) {
            foreach ($data as $key => $val) {
                $update_value[] = "$key='{$val}'";
            }
            $update_value = implode(",", $update_value);
        }

        $sql = "UPDATE ".$table." SET ".$update_value." WHERE ".$where;
        $result = $this->conn->query($sql);

        return $result ? true : false;
    }
    
    public function delete($table, $filter) {
        $sql = "DELETE FROM ".$table." ".$filter;
        $result = $this->conn->query($sql);

        return $result ? true : false;
    }
}
?>
```

## 7. Kesimpulan

Pada praktikum ini, mahasiswa telah mempelajari:

Konsep OOP: class, object, property, method.

Constructor dan enkapsulasi.

Membuat class library (Form & Database).

Menggunakan modularisasi dengan include.

Membangun aplikasi sederhana dengan input form dan database MySQL.
