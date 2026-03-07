#lab 1 :Reflected Xss into HTML context with nothing encoded :

Vulnerability : There is simple xss vulnerablity in search functionality.

End Goal :
We call alert function as POC.

Solution :
(i) In this fucntionality we run simple script  and call inside alert function in it.
  Payload : <script>alert(1)</script>