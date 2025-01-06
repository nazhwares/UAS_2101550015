# UAS_2101550015
NAZHWA RESTA
### login.php ###
```
<?php
session_start();
require_once 'config.php';

if(isset($_POST['login'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];
    
    // Gunakan fungsi PASSWORD() untuk mencocokkan dengan hash di database
    $query = "SELECT * FROM users WHERE username='$username' AND password=PASSWORD('$password')";
    $result = mysqli_query($conn, $query);
    
    if(mysqli_num_rows($result) == 1) {
        $row = mysqli_fetch_assoc($result);
        $_SESSION['user_id'] = $row['id'];
        $_SESSION['username'] = $row['username'];
        $_SESSION['name'] = $row['name'];
            
        header("Location: dashboard.php");
        exit();
    } else {
        $error = "Username atau Password salah!";
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Login</title>
   <style>
    body {
            background-color: #b3e0ff; /* Warna latar belakang biru muda */
            background-image: url('path/to/your/cake-shop-background.jpg'); /* Ganti dengan path gambar Anda */
            background-size: cover; /* Mengatur gambar agar menutupi seluruh latar belakang */
            background-position: center; /* Memusatkan gambar */
            font-family: Arial, sans-serif; /* Font yang lebih modern */
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh; /* Mengisi tinggi viewport */
            margin: 0;
            color: white; /* Mengubah warna teks menjadi putih untuk kontras yang lebih baik */
        }
        .login-container {
            background-color: rgba(255, 255, 255, 0.9); /* Warna latar belakang untuk formulir dengan transparansi */
            padding: 20px;
            border-radius: 8px; /* Sudut yang membulat */
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2); /* Bayangan untuk efek kedalaman */
            width: 300px; /* Lebar tetap untuk formulir */
        }
        h1 {
            text-align: center; /* Pusatkan judul */
            color: #333; /* Warna teks judul */
            margin-bottom: 20px; /* Jarak bawah judul */
        }
        p {
            margin: 10px 0; /* Jarak antar elemen */
        }
        input[type="text"],
        input[type="password"] {
            width: 100%; /* Lebar penuh */
            padding: 10px; /* Padding untuk ruang dalam */
            border: 1px solid #ccc; /* Border abu-abu */
            border-radius: 5px; /* Sudut yang membulat */
            box-sizing: border-box; /* Menghitung padding dan border dalam lebar */
        }
        input[type="submit"] {
            background-color: #007bff; /* Warna latar belakang tombol */
            color: white; /* Warna teks tombol */
            border: none; /* Tanpa border */
            padding: 10px; /* Padding untuk tombol */
            border-radius: 5px; /* Sudut yang membulat */
            cursor: pointer; /* Kursor pointer saat hover */
            width: 100%; /* Lebar penuh */
            transition: background-color 0.3s; /* Transisi untuk efek hover */
        }
        input[type="submit"]:hover {
            background-color: #0056b3; /* Warna saat hover */
        }
        .error {
            color: red; /* Warna merah untuk pesan error */
            text-align: center; /* Pusatkan pesan error */
        }
    </style>
</head>
<body>
<div class="login-container">
        <h1>Login Form</h1> <!-- Judul dipindahkan ke sini -->
        <?php if(isset($error)) { ?>
            <p class="error"><?php echo $error; ?></p>
        <?php } ?>
    
    <form action="" method="POST">
        <p>
            Username: <input type="text" name="username" required>
        </p>
        <p>
            Password: <input type="password" name="password" required>
        </p>
        <p>
            <input type="submit" name="login" value="Login">
        </p>
    </form>
</body>
</html>
```
### config.php ###
```
<?php
$host = "localhost";
$dbuser = "root";
$dbpass = "";
$dbname = "login_db";

$conn = mysqli_connect($host, $dbuser, $dbpass, $dbname);
if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}
?>
```
### dashboard.php ###
```
<?php
session_start();

// Cek apakah user sudah login
if(!isset($_SESSION['user_id'])) {
    header("Location: login.php");
    exit();
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daftar Kue</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #b3e0ff; /* Warna latar belakang biru muda */
        }
        h1 {
            color: #343a40;
            margin-bottom: 20px;
        }
        table {
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            overflow: hidden;
            background-color: #ffffff; /* Warna latar belakang untuk tabel */
        }
        th, td {
            text-align: center;
        }
        .btn-group-action {
            white-space: nowrap;
        }
        .btn {
            transition: background-color 0.3s, transform 0.3s;
        }
        .btn:hover {
            transform: translateY(-2px);
        }
        .modal-content {
            border-radius: 8px;
            background-color: #f8f9fa; /* Warna latar belakang untuk modal */
        }
        .modal-header {
            background-color: #007bff;
            color: white;
        }
        .form-control {
            border-radius: 5px;
        }
        .form-control:focus {
            box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
            border-color: #80bdff;
        }
        .modal-footer .btn {
            border-radius: 5px;
        }
        .navbar-brand img {
            height: 40px; /* Set the height of the logo */
        }
    </style>
</head>
<body class="container py-4">
    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light mb-4">
        <div class="container-fluid">
            <a class="navbar-brand" href="#">
                <img src="logo.jpg" alt="Logo"> <!-- Replace with your logo path -->
                Melow Bakery
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="dashboard.php">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="logout.php">Logout</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <!-- Image -->
    <div class="text-center mb-4">
        <img src="roti.jpg" alt="Delicious Cakes" class="img-fluid" style="width: 100%; height: 300px;"> <!-- Replace with your image path -->
    </div>

    <h1>Daftar Kue</h1>
    
    <div class="row mb-3">
        <div class="col">
            <input type="text" id="searchInput" class="form-control" placeholder="Cari berdasarkan ID">
        </div>
        <div class="col-auto">
            <button onclick="searchCake()" class="btn btn-primary">Cari</button>
        </div>
        <div class="col-auto">
            <button type="button" class="btn btn-success" data-bs-toggle="modal" data-bs-target="#cakeModal">
                Tambah Kue
            </button>
        </div>
    </div>

    <table class="table table-striped">
        <thead class="table-dark">
            <tr>
                <th>ID</th>
                <th>Nama</th>
                <th>Rasa</th>
                <th>Harga</th>
                <th> Stok</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody id="cakeList">
        </tbody>
    </table>

    <!-- Modal untuk Tambah/Edit Kue -->
    <div class="modal fade" id="cakeModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="modalTitle">Tambah Kue</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <form id="cakeForm">
                        <input type="hidden" id="cakeId">
                        <div class="mb-3">
                            <label for="name" class="form-label">Nama</label>
                            <input type="text" class="form-control" id="name" required>
                        </div>
                        <div class="mb-3">
                            <label for="flavor" class="form-label">Rasa</label>
                            <input type="text" class="form-control" id="flavor" required>
                        </div>
                        <div class="mb-3">
                            <label for="price" class="form-label">Harga</label>
                            <input type="number" class="form-control" id="price" required>
                        </div>
                        <div class="mb-3">
                            <label for="stock" class="form-label">Stok</label>
                            <input type="number" class="form-control" id="stock" required>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
                    <button type="button" class="btn btn-primary" onclick="saveCake()">Simpan</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        const API_URL = 'http://localhost/cakes/cakes.php';
        let cakesModal;

        document.addEventListener('DOMContentLoaded', function() {
            cakesModal = new bootstrap.Modal(document.getElementById('cakeModal'));
            loadCakes(); // Memuat semua kue saat halaman pertama kali dibuka
        });

        function loadCakes() {
            fetch(API_URL)
                .then(response => response.json())
                .then(cakes => {
                    const cakeList = document.getElementById('cakeList');
                    cakeList.innerHTML = '';
                    cakes.forEach(cake => {
                        cakeList.innerHTML += 
                            `<tr>
                                <td>${cake.id}</td>
                                <td>${cake.name}</td>
                                <td>${cake.flavor}</td>
                                <td>${cake.price}</td>
                                <td>${cake.stock}</td>
                                <td class="btn-group-action">
                                    <button class="btn btn-sm btn-warning me-1" onclick="editCake(${cake.id})">Edit</button>
                                    <button class="btn btn-sm btn-danger" onclick="deleteCake(${cake.id})">Hapus</button>
                                </td>
                            </tr>`;
                    });
                })
                .catch(error => alert('Error loading cakes: ' + error));
        }

        function searchCake() {
            const id = document.getElementById('searchInput').value.trim();
            if (!id) {
                loadCakes(); // Jika input ID kosong, tampilkan semua kue
                return;
            }

            // URL untuk mencari kue berdasarkan ID
            const url = `${API_URL}/${id}`;

            fetch(url)
                .then(response => response.json())
                .then(cake => {
                    const cakeList = document.getElementById('cakeList');
                    cakeList.innerHTML = '';

                    if (cake.message) {
                        alert('Cake not found');
                        return;
                    }

                    cakeList.innerHTML = 
                        `<tr>
                            <td>${cake.id}</td>
                            <td>${cake.name}</td>
                            <td>${cake.flavor}</td>
                            <td>${cake.price}</td>
                            <td>${cake.stock}</td>
                            <td class="btn-group-action">
                                <button class="btn btn-sm btn-warning me-1" onclick="editCake(${cake.id})">Edit</button>
                                <button class="btn btn-sm btn-danger" onclick="deleteCake(${cake.id})">Hapus</button>
                            </td>
                        </tr>`;
                })
                .catch(error => alert('Error searching cake: ' + error));
        }

        function editCake(id) {
            fetch(`${API_URL}/${id}`)
                .then(response => response.json())
                .then(cake => {
                    document.getElementById('cakeId').value = cake.id;
                    document.getElementById('name').value = cake.name;
                    document.getElementById('flavor').value = cake.flavor;
                    document.getElementById('price').value = cake.price;
                    document.getElementById('stock').value = cake.stock;
                    document.getElementById('modalTitle').textContent = 'Edit Kue';
                    cakesModal.show();
                })
                .catch(error => alert('Error loading cake details: ' + error));
        }

        function deleteCake(id) {
            if (confirm('Are you sure you want to delete this cake?')) {
                fetch(`${API_URL}/${id}`, { method: 'DELETE' })
                    .then(response => response.json())
                    .then(data => {
                        alert('Cake deleted successfully');
                        loadCakes();
                    })
                    .catch(error => alert('Error deleting cake: ' + error));
            }
        }

        function saveCake() {
            const cakeId = document.getElementById('cakeId').value;
            const cakeData = {
                name: document.getElementById('name').value,
                flavor: document.getElementById('flavor').value,
                price: document.getElementById('price').value,
                stock: document.getElementById('stock').value
            };

            const method = cakeId ? 'PUT' : 'POST';
            const url = cakeId ? `${API_URL}/${cakeId}` : API_URL;

            fetch(url, {
                method: method,
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(cakeData)
            })
            .then(response => response.json())
            .then(data => {
                alert(cakeId ? 'Cake updated successfully' : 'Cake added successfully');
                cakesModal.hide();
                loadCakes();
                resetForm();
            })
            .catch(error => alert('Error saving cake: ' + error));
        }

        function resetForm() {
            document.getElementById('cakeId').value = '';
            document.getElementById('cakeForm').reset();
            document.getElementById('modalTitle').textContent = 'Tambah Kue';
        }

        // Reset form when modal is closed
        document.getElementById('cakeModal').addEventListener('hidden.bs.modal', resetForm);
    </script>
</body>
</html>
```
### logout.php ###
```
<?php
session_start();

// Hapus semua data session
session_destroy();

// Redirect ke halaman login
header("Location: login.php");
exit();
?>

```
