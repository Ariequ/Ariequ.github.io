---
layout: post
title: "snake_preview"
date: 2015-01-03 21:29:09 +0800
comments: true
categories: 
---

<div>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
	<title>Unity Web Player | Parachute</title>
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
		var config = {
			width: 320, 
			height: 568,
			params: { enableDebugging:"0" }
			,baseDownloadUrl: "http://webplayer.unity3d.com/download_webplayer-3.x/"
		};
		var u = new UnityObject2(config);

		jQuery(function() {

			var $missingScreen = jQuery("#unityPlayer").find(".missing");
			var $brokenScreen = jQuery("#unityPlayer").find(".broken");
			$missingScreen.hide();
			$brokenScreen.hide();
			
			u.observeProgress(function (progress) {
				switch(progress.pluginStatus) {
					case "broken":
						$brokenScreen.find("a").click(function (e) {
							e.stopPropagation();
							e.preventDefault();
							u.installPlugin();
							return false;
						});
						$brokenScreen.show();
					break;
					case "missing":
						$missingScreen.find("a").click(function (e) {
							e.stopPropagation();
							e.preventDefault();
							u.installPlugin();
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
			u.initPlugin(jQuery("#unityPlayer")[0], "/unity/snake.unity3d");
		});
	-->
	</script>
</div>
<div>
	<div class="content">
		<div id="unityPlayer">
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