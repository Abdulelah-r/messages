 <!DOCTYPE html>
<html>
<head>
  <title>Message Board</title>
</head>
<body>
  <h1>Message Board</h1>
  <?php
   
      
      @$db = new mysqli('localhost', 'messageWebUser', 'Amn505050', 'MessageDB');
  
 if (mysqli_connect_errno()){
    echo '<p>Error: Could not connect to database. </br> 
   Please try again later. <p>';
    exit;
  }
     
      $query = "SELECT Users.FirstName, Users.LastName,
      Messages.UserID AS UserLikeID, Messages.MessageID, Messages.Message, Messages.Date, 
      Messages.Time, COUNT(UserMessageLikes.UserID) AS Likes
      FROM Users INNER JOIN  Messages 
      ON Users.UserID = Messages.UserID  
      LEFT JOIN UserMessageLikes 
      ON Messages.MessageID = UserMessageLikes.MessageID
      GROUP BY Messages.MessageID
      ORDER BY COUNT(UserMessageLikes.UserID) DESC";  
      
      $stmt = $db->prepare($query);  
         $stmt->execute(); 
          $stmt->store_result();
      
      $stmt->bind_result($firstName, $lastName, $userID, $messageID, $message, $date, $time, $likes);

      
     echo "<p>Number of messages found: ".$stmt->num_rows."</p>"; 

     
      while($stmt->fetch()) {
        
        echo '<p><strong><a href="userMessages.php?UserID=' . $userID . '">' .$firstName . ' ' . $lastName . ' ' .  '</a>' ;
        "<br />";     
        echo '<a href="messagelikes.php?MessageID=' . $messageID . '">' . $message . ":</a>";
        echo  "<br />Date: ". $date . " Time: ".$time. " Likes: ".$likes. "</p></strong>";
      
      }
  
  $stmt->free_result();
  $db->close();
     
  ?>
</body>
</html>

