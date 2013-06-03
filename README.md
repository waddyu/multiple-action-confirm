multiple-action-confirm
=======================

multiple action confirm box in Javascript.


標準のconfirmだと、「はい」と「キャンセル」しかないので、「複数の選択肢を表示し、ユーザの選択に応じたアクションをとる」という操作がパパっと実装できるとよいですね。  
GitHub公開中：https://github.com/yu-wasama/multiple-action-confirm

[ jQuery Confirm Dialog Replacement ](http://tutorialzine.com/2010/12/better-confirm-box-jquery-css3/ " jQuery Confirm Dialog Replacement")をベースにさせていただいています。

##ソース

```js:javascripts/maconfirm.js
var uploadConfirm;

uploadConfirm = function(params) {
  var buttonHTML, buttons, confirmOverlay, i, markup;
  buttonHTML = "";
  $.each(params.buttons, function(name, obj) {
    buttonHTML += "<a href=\"#\" class=\"button " + obj['class'] + "\">" + name + "<span></span></a>";
    return obj.action || (obj.action = function() {});
  });
  confirmOverlay = "confirmOverlay" + params.id;
  markup = ("<div id=\"" + confirmOverlay + "\" class=\"confirmOverlay\">") + '<div id="confirmBox" class="hero-unit">' + '<h6>' + params.title + '</h6>' + '<p><small>' + params.message + '</small></p>' + '<div id="confirmButtons">' + buttonHTML + '</div></div></div>';
  $(markup).hide(confirmOverlay).appendTo("body");
  buttons = $('#confirmBox').find('.button');
  i = 0;
  return $.each(params.buttons, function(name, obj) {
    return buttons.eq(i++).click(function() {
      obj.action();
      $("#" + confirmOverlay).remove();
      return false;
    });
  });
};
```

```css:stylesheets/maconfirm.css
.confirmOverlay{
	width:100%;
	height:100%;
	position:fixed;
	top:0;
	left:0;
	background: rgba ( 33, 33, 33, 0.6 );
	background: url('../images/ie.png');
	z-index: 100000;
}

#confirmBox{
	width:	520px;
	margin:	-90px 0 0 -260px;
	border: 180px;
	padding: 3px;
	position:fixed;
	left:50%;
	top:50%;
}

#confirmButtons a {
	margin: 3px;
	width: 21%;
	padding: 2px 5px;
}

#confirmBox	p{
	margin: 10px;
}

#confirmBox h6 {
	background-color: cornflowerblue;
	margin:	3px;
	border-radius: 3px;
	-moz-border-radius: 3px;
	padding-left:	5px;
	color:	white;
}
```

## 使い方
```html:confirmPage.html
<!DOCTYPE html>

<html>
	<head>
		<title>Multiple Button Confirm</title>
		<script src="../multiple-action-confirm-box/javascripts/maconfirm.js" type="text/javascript"></script>
		<script src="http://code.jquery.com/jquery-1.9.1.js" type="text/javascript"></script>
		<link rel="stylesheet" media="screen" href="../multiple-action-confirm-box/stylesheets/maconfirm.css">
		<link href="http://netdna.bootstrapcdn.com/twitter-bootstrap/2.3.2/css/bootstrap-combined.min.css" rel="stylesheet">
		
	</head>
	<body>
		<a href="javascript:void(0);" id="confirmLink">Multiple Button Confirm Link</a>
	</body>
	<script type="text/javascript">
		$(function(){
			$("#confirmLink").click(function(){
				uploadConfirm({
				    id: 'confirm',
				    title: "override confirm",
				    message: 'We have a same name file already. Do you override it?',
				    buttons: {
				      "Yes": {
				        "class": "btn btn-info btn-small",
				        action: function() {
				        	alert('replaced a file.');
				        }
				      },
				      "Yes To All": {
				        "class": "btn btn-info btn-small",
				        action: function() {
				        	alert('replaced all files.');
				        }
				      },
				      "No": {
				        "class": "btn btn-small",
				        action: function() {
				        	alert('original')
				        }
				      },
				      "No To All": {
				        "class": "btn btn-small",
				        action: function() {
				        	alert('original')
				        }
				      }
				    }
				  });
			});
		});
	</script>
</html>
```
選択肢それぞれの"action"プロパティに任意の処理をセットすることで、ユーザの選択に応じて望みの処理を実行することができます。
