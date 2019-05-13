---
title: "Experiment TEST"
date: 2019-02-20T16:22:09+01:00
draft: false
---

Test
<form>
    <label for="pswd">Enter your password: </label>
    <input type="password" id="pswd">
    <input type="button" value="Submit" onclick="checkPswd();" />
</form>
<!--Function to check password the already set password is admin-->
<script type="text/javascript">
    function checkPswd() {
        var confirmPassword = "admin";
        var password = document.getElementById("pswd").value;
        if (password == confirmPassword) {
             window.location="index.html";
        }
        else{
            alert("Passwords do not match.");
        }
    }
</script>
