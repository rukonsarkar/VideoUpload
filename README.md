### Video upload to server and play in the app.
১) উপরের Code এ ক্লিক করে Download Zip এ ক্লিক করে Zip ফাইল ডাউনলোড করুন। তারপর Unzip করে Android studio তে ওপেন করুন।

২) Database এর মধ্যে video_table নামে একটা Table Create করুন। Column এর নাম দিন id, title, video. যেমনঃ

![mysql](https://github.com/user-attachments/assets/1be13658-3ccd-4bbb-943d-943f3be3d6cd)

৩) cPanel এ গিয়ে File Manager এর মধ্যে গিয়ে public_html এর ভিতরে গিয়ে Natok নাম দিয়ে একটা Folder Create করুন। এই Folder এর ভিতরে আরেকটা Folder Create করুন নাম দিনঃ videos এবং Natok Folder এর ভিতরে upload_video.php এবং get_video.php নাম দিয়ে দুইটা PHP ফাইল Create করুন। যেমনঃ

![filemanager](https://github.com/user-attachments/assets/37071f2d-6f8c-4c2c-9db6-7689a3e919f2)

> ৪) **upload_video.php** Code: এই কোডের মাধ্যমে সার্ভারে ভিডিও আপলোড করা হয়েছেঃ
```
<?php
 $con = mysqli_connect('localhost', 'teqleoch_sms_user', 'Sms@2001$', 'teqleoch_sms_db');

 $ACTION = $_POST['action'];
 $fileData = $_POST['file'];
 $TITLE = $_POST['title'];

if($ACTION == "action"){

	$FILENAME = "VIDEO_".uniqid().".mp4";
	
	file_put_contents("videos/".$FILENAME,base64_decode($fileData));

	$query = "INSERT INTO `video_table` (`title`, `video`) VALUES ('$TITLE', '$FILENAME')";

	$result = mysqli_query($con, $query);

	if($result){
		echo "successful";
	} else {
		echo "failure";
	}
	mysqli_close($con);
}
?>
```

> 5) **get_video.php** Code: এই কোডের মাধ্যমে সার্ভার থেকে লাস্ট ভিডিও Retrive করে অ্যাপের মধ্যে আনা হয়েছেঃ 
```
<?php
header('Content-Type: application/json; charset=utf-8');
$con = mysqli_connect('localhost', 'teqleoch_sms_user', 'Sms@2001$', 'teqleoch_sms_db');
 
$select = "SELECT * FROM video_table ORDER BY id DESC LIMIT 1";

$result = mysqli_query($con, $select);

$data = array();

foreach($result as $item){
$id = $item['id'];
$title = $item['title'];
$video = $item['video'];

$list['id'] = $id;
$list['title'] = $title;
$list['video'] = $video;
	array_push($data, $list);
}
	echo json_encode($data);
	mysqli_close($con);
?>
```

৬) আপনি যদি সব গুলো ভিডিও সার্ভার থেকে Get করে Json আকারে পেতে চান তাহলে এই php code use করুনঃ
```
<?php
header('Content-Type: application/json; charset=utf-8');
$con = mysqli_connect('localhost', 'teqleoch_sms_user', 'Sms@2001$', 'teqleoch_sms_db');
 
$select = "SELECT * FROM video_table";

$result = mysqli_query($con, $select);

$data = array();

foreach($result as $item){
$id = $item['id'];
$title = $item['title'];
$video = $item['video'];

$list['id'] = $id;
$list['title'] = $title;
$list['video'] = $video;
	array_push($data, $list);
}
	echo json_encode($data);
	mysqli_close($con);
?>
```
নোটঃ PHP কোডের মধ্যে আপনার Database Name, User name, Password দিয়ে Repleace করে নিবেন!

- [F R. RUKON SARKAR](https://www.facebook.com/programmerrukon/) - App Developer 
