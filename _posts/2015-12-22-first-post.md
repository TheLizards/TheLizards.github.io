---
layout: post
title: First Post Woot Woot
date:   2015-12-22 12:44:58 -0500
categories: jekyll update
published: true
highlighter: rouge
---

{% highlight php linenos %}
<?php
// If user_id session variable is not set, redirect user
if (!isset($_SESSION['user_id']))
{
	
	$url = '../forum_index.php'; // Define the URL.
	ob_end_clean(); // Delete the buffer.
	header("Location: $url");
	exit(); // Quit the script.
	
}
?>
{% endhighlight %}