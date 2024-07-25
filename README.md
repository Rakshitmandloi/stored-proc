<!doctype html>
<html>
<head>
    <title>Fetch Windows Username</title>
</head>
<body>
<script type="text/javascript">
    function getUsername() {
        try {
            var WinNetwork = new ActiveXObject("WScript.Network");
            return WinNetwork.UserName;
        } catch (e) {
            console.error("ActiveXObject not supported.");
            return "Unknown User";
        }
    }

    window.localStorage.setItem("username", getUsername());
</script>
</body>
</html>
