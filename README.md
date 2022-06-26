# gamelist
<!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->
<html lang="en">
<head>
<title>Video Games</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet"
href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<script
src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script
src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
</head>
<body>
<div class="container">
<h2>Game name and Type</h2>
<form id="gameForm" method="post">
<div class="form-group">
<span><label for="gamename">Game Name:</label> <label id="gamenameMsg">
</label></span>
<input type="text" class="form-control" name="GameName" id="gamename"
placeholder="Enter Game Name" required>
</div>
<div class="form-group">
<label for="gamegenre">Game Genre:</label>
<input type="text" class="form-control" id="gamegenre"
placeholder="Enter Genre of the game" name="gamegenre">
</div>
<div class="form-group">
<label for="gamelike">Game Like:</label>
<input type="text" class="form-control" id="gamelike"
placeholder="Enter your choice" name="gamelike">
</div>
<input type="button" class="btn btn-primary" id="GameSave" value="Save"
onclick="saveGame();">
</form>
</div>
<script>
$("#gamename").focus();
function validateAndGetFormData() {
var gamenameVar = $("#gamename").val();
if (gamenameVar === "") {
alert("Game name not entered");
$("#gamename").focus();
return "";
}
var gamegenreVar = $("#gamegenre").val();
if (gamegenreVar === "") {
alert("Genre not entered");
$("#gamegenre").focus();
return "";
}
var gamelikeVar = $("#gamelike").val();
if (gamelikeVar === "") {
alert("Choice not entered");
$("#gamelike").focus();
return "";
}
var jsonStrObj = {
gamename: gamenameVar,
gamegenre: gamegenreVar,
gamelike: gamelikeVar
};
return JSON.stringify(jsonStrObj);
}
// This method is used to create PUT Json request.
function createPUTRequest(connToken, jsonObj, dbName, relName) {
var putRequest = "{\n"
+ "\"token\" : \""
+ connToken
+ "\","
+ "\"dbName\": \""
+ dbName
+ "\",\n" + "\"cmd\" : \"PUT\",\n"
+ "\"rel\" : \""
+ relName + "\","
+ "\"jsonStr\": \n"
+ jsonObj
+ "\n"
+ "}";
return putRequest;
}
function executeCommand(reqString, dbBaseUrl, apiEndPointUrl) {
var url = dbBaseUrl + apiEndPointUrl;
var jsonObj;
$.post(url, reqString, function (result) {
jsonObj = JSON.parse(result);
}).fail(function (result) {
var dataJsonObj = result.responseText;
jsonObj = JSON.parse(dataJsonObj);
});
return jsonObj;
}
function resetForm() {
$("#gamename").val("");
$("#gamegenre").val("");
$("#gamelike").val("");
$("#gamename").focus();
}
function saveGame() {
var jsonStr = validateAndGetFormData();
if (jsonStr === "") {
return;
}
var putReqStr = createPUTRequest("90939357|-31949287514643522|90939605",
jsonStr, "Video Games", "Direct");
alert(putReqStr);
jQuery.ajaxSetup({async: false});
var resultObj = executeCommand(putReqStr,
"http://api.login2explore.com:5577", "/api/iml");
alert(JSON.stringify(resultObj));
jQuery.ajaxSetup({async: true});
resetForm();
}
</script>
</body>
</html>
