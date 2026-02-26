<?php
/* ================= SESSION & DATABASE ================= */
session_start();

$conn = new mysqli("localhost", "root", "arv123arv", "railway");
if ($conn->connect_error) {
    die("Connection failed");
}

$page = isset($_GET['page']) ? $_GET['page'] : 'home';
?>

<!DOCTYPE html>
<html>
<head>
    <title>Railway Reservation System</title>
</head>

<body style="
    background-image: url(adminlogin.jpeg);
    height: 100%;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
">

<div align="center">

<?php
/* ================= HOME ================= */
if ($page == "home") {
?>
    <h2>Railway Reservation System</h2>
    <a href="?page=admin">Admin Login</a><br><br>
    <a href="?page=enquiry">Train Enquiry</a>
<?php
}

/* ================= ADMIN LOGIN ================= */
if ($page == "admin") {

    if (isset($_POST['uid'], $_POST['password'])) {
        if ($_POST['uid'] == "admin" && $_POST['password'] == "admin") {
            $_SESSION['admin'] = true;
        }
    }

    if (isset($_SESSION['admin'])) {
?>
        <h3>Admin Menu</h3>
        <a href="?page=booked">View Booked Tickets</a><br><br>
        <a href="?page=cancelled">View Cancelled Tickets</a><br><br>
        <a href="?page=logout">Logout</a>
<?php
    } else {
?>
        <h3>Admin Login</h3>
        <form method="post">
            User ID:
            <input type="text" name="uid" required><br><br>

            Password:
            <input type="password" name="password" required><br><br>

            <input type="submit" value="Login">
        </form>
<?php
    }
}

/* ================= LOGOUT ================= */
if ($page == "logout") {
    session_destroy();
    echo "<h3>Logged out successfully</h3>";
    echo '<a href="?page=home">Go Home</a>';
}

/* ================= BOOKED TICKETS ================= */
if ($page == "booked" && isset($_SESSION['admin'])) {

    $res = $conn->query("SELECT * FROM resv WHERE status='BOOKED'");

    echo "<h3>Booked Tickets</h3>";
    echo "<table border='1'>
            <tr>
                <th>PNR</th>
                <th>User ID</th>
                <th>Train No</th>
                <th>Date</th>
                <th>Fare</th>
                <th>Class</th>
                <th>Seats</th>
                <th>Status</th>
            </tr>";

    while ($row = $res->fetch_array()) {
        echo "<tr>
                <td>$row[0]</td>
                <td>$row[1]</td>
                <td>$row[2]</td>
                <td>$row[5]</td>
                <td>$row[6]</td>
                <td>$row[7]</td>
                <td>$row[8]</td>
                <td>$row[9]</td>
              </tr>";
    }
    echo "</table><br>";
    echo '<a href="?page=admin">Back</a>';
}

/* ================= CANCELLED TICKETS ================= */
if ($page == "cancelled" && isset($_SESSION['admin'])) {

    $res = $conn->query("SELECT * FROM resv WHERE status='CANCELLED'");

    echo "<h3>Cancelled Tickets</h3>";
    echo "<table border='1'>
            <tr>
                <th>PNR</th>
                <th>User ID</th>
                <th>Train No</th>
                <th>Date</th>
                <th>Fare</th>
                <th>Class</th>
                <th>Seats</th>
                <th>Status</th>
            </tr>";

    while ($row = $res->fetch_array()) {
        echo "<tr>
                <td>$row[0]</td>
                <td>$row[1]</td>
                <td>$row[2]</td>
                <td>$row[5]</td>
                <td>$row[6]</td>
                <td>$row[7]</td>
                <td>$row[8]</td>
                <td>$row[9]</td>
              </tr>";
    }
    echo "</table><br>";
    echo '<a href="?page=admin">Back</a>';
}

/* ================= TRAIN ENQUIRY ================= */
if ($page == "enquiry") {
?>
    <h3>Train Enquiry</h3>
    <form method="post" action="?page=enquiry_result">

        Starting Point:
        <select name="sp" required>
            <option></option>
            <?php
            $s = $conn->query("SELECT sname FROM station");
            while ($r = $s->fetch_assoc()) {
                echo "<option>{$r['sname']}</option>";
            }
            ?>
        </select><br><br>

        Destination Point:
        <select name="dp" required>
            <option></option>
            <?php
            $s = $conn->query("SELECT sname FROM station");
            while ($r = $s->fetch_assoc()) {
                echo "<option>{$r['sname']}</option>";
            }
            ?>
        </select><br><br>

        Date of Journey:
        <input type="date" name="doj" required><br><br>

        <input type="submit" value="Search">
    </form>
<?php
}

/* ================= ENQUIRY RESULT ================= */
if ($page == "enquiry_result") {

    $sp  = $_POST['sp'];
    $dp  = $_POST['dp'];
    $doj = $_POST['doj'];

    $q = $conn->query("
        SELECT t.trainno, t.tname, c.class, c.fare, c.seatsleft
        FROM train t, classseats c
        WHERE t.trainno = c.trainno
        AND c.sp = '$sp'
        AND c.dp = '$dp'
        AND c.doj = '$doj'
    ");

    echo "<h3>Available Trains</h3>";

    if ($q->num_rows == 0) {
        echo "No trains found";
    } else {
        echo "<table border='1'>
                <tr>
                    <th>Train No</th>
                    <th>Train Name</th>
                    <th>Class</th>
                    <th>Fare</th>
                    <th>Seats Left</th>
                </tr>";

        while ($r = $q->fetch_array()) {
            echo "<tr>
                    <td>$r[0]</td>
                    <td>$r[1]</td>
                    <td>$r[2]</td>
                    <td>$r[3]</td>
                    <td>$r[4]</td>
                  </tr>";
        }
        echo "</table>";
    }

    echo "<br><a href='?page=enquiry'>More Enquiry</a>";
}

$conn->close();
?>

<br><br>
<a href="?page=home">Home Page</a>

</div>
</body>
</html>
