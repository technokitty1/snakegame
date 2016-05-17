# snakegame

<html>
<head>
	<title>Database Images</title>
	<link rel="stylesheet" href="https://bootswatch.com/slate/bootstrap.min.css">
</head>
<body>
	<div class=jumbotron>
		<h3>How to Load an Image Reference</h3>
	</div>

	<?php
		$servername = "localhost";
		$username = "root";
		$password = "";
		$dbname = "bonusdb";

		# Create connection
		$conn = new mysqli($servername, $username, $password, $dbname);
		# Check connection
		if ($conn->connect_error) {
		    die("Connection failed: " . $conn->connect_error);
		}

        $sql = "SELECT name, image FROM product";
        $result = $conn->query($sql);
        
        if ($result->num_rows > 0) {
            // output data of each row
            while($row = $result->fetch_assoc()) {
                echo "<div class='col-md-3 col-sm-6 hero-feature'>
					    <div class='thumbnail'>
					    <img src='Images/" . $row['image'] . "' alt=''>
						    <div class='caption'>
							    <h3>" . $row['name'] . "</h3>
							    <p>
								    <a href='#' class='btn btn-success'>Buy Now!</a>
								    <a href='#' class='btn btn-info'>More Info</a>
							    </p>
						    </div>
					    </div>
				    </div>";
            }
        } 
        else {
            echo "No Products To Show";
        }
        $conn->close();
    ?>
</body>
</html>
