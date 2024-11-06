---
title: Quick Hitter - mysqli vs PDO
date: 2013-10-23T06:50:42.000Z
description: Quick Hitter - mysqli vs PDO
---

I wanted to put up a quick post to run down something surprising I came across today at work. While writing a login for my company’s back end I was faced with a choice. I had to decide between using mysqli or PDO to prepare and execute the SQL queries for the user name and login information. I probably didn’t do enough research here but I went with mysqli. Lately I have been working on several functions that needed to query the database and return several values. I had been working with the procedural style of mysqli but today I decided to divert and go with the object oriented style. Everything worked fantastically until I got my query results and needed to throw those values into an array. Turns out, using mysqli you cannot do this unless you manually loop through the results and push them to the array. This kind of blew me away. If you are using the object oriented style of PDO to do this it’s really easy to do something like the following:

 $stmt = $db->prepare("SELECT val1, val2 FROM table WHERE col = $someuserinput"); $stmt->execute(); $result = $stmt->fetch(PDO::FETCH_ASSOC);

It’s worth noting that this will fetch the associative array but you can also return an object or as a numerically indexed array. I think moving forward I’ll look into some other differences but this is a glaring one. It would seem to be a no brainer to work this functionality in based on the amount of data typically stored in a modern site’s database and the need to recall that data efficiently. The more you know kids…the more you know.

**UPDATE:**

I was confused from the comments I was reading on the php manual. There is an equivalent here, you can use [fetch_array](http://php.net/manual/en/mysqli-result.fetch-array.php "PHP.net mysqli fetch array"). User inputs need to be sanitized through mysqli real escape string instead of the prepare/excute method. It would look something like this:

 $input = $mysqli->real_escape_string($someuserinput); $result = $mysqli->query("SELECT val1, val2 FROM table WHERE col = {$input}"); $data = $result->Fetch_array(MYSQLI_ASSOC);


