---
layout: post
title: "BasketBallCourt_Fight"
date: 2014-10-11 00:23:24 +0800
comments: true
categories: 
---

<div>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
	<title>BasketBallCourtFight</title>
	<script type='text/javascript' src='https://ssl-webplayer.unity3d.com/download_webplayer-3.x/3.0/uo/jquery.min.js'></script>
	<script type="text/javascript">
	<!--
	var unityObjectUrl = "http://webplayer.unity3d.com/download_webplayer-3.x/3.0/uo/UnityObject2.js";
	if (document.location.protocol == 'https:')
		unityObjectUrl = unityObjectUrl.replace("http://", "https://ssl-");
	document.write('<script type="text\/javascript" src="' + unityObjectUrl + '"><\/script>');
	-->
	</script>
	<script type="text/javascript">
	<!--
		var config2 = {
			width: 750, 
			height: 500,
			params: { enableDebugging:"0" }
			,baseDownloadUrl: "http://webplayer.unity3d.com/download_webplayer-3.x/"
		};
		var u2 = new UnityObject2(config2);

		jQuery(function() {

			var $missingScreen = jQuery("#unityPlayer2").find(".missing");
			var $brokenScreen = jQuery("#unityPlayer2").find(".broken");
			$missingScreen.hide();
			$brokenScreen.hide();
			
			u2.observeProgress(function (progress) {
				switch(progress.pluginStatus) {
					case "broken":
						$brokenScreen.find("a").click(function (e) {
							e.stopPropagation();
							e.preventDefault();
							u2.installPlugin();
							return false;
						});
						$brokenScreen.show();
					break;
					case "missing":
						$missingScreen.find("a").click(function (e) {
							e.stopPropagation();
							e.preventDefault();
							u2.installPlugin();
							return false;
						});
						$missingScreen.show();
					break;
					case "installed":
						$missingScreen.remove();
					break;
					case "first":
					break;
				}
			});
			u2.initPlugin(jQuery("#unityPlayer2")[0], "/unity/BasketBallCourtFight.unity3d");
		});
	-->
	</script>
</div>
<div>
	<div class="content">
		<div id="unityPlayer2">
			<div class="missing">
				<a href="http://unity3d.com/webplayer/" title="Unity Web Player. Install now!">
					<img alt="Unity Web Player. Install now!" src="http://webplayer.unity3d.com/installation/getunity.png" width="193" height="63" />
				</a>
			</div>
			<div class="broken">
				<a href="http://unity3d.com/webplayer/" title="Unity Web Player. Install now! Restart your browser after install.">
					<img alt="Unity Web Player. Install now! Restart your browser after install." src="http://webplayer.unity3d.com/installation/getunityrestart.png" width="193" height="63" />
				</a>
			</div>
		</div>
	</div>
</div>
