<!-- F28443F2EBA025B60FF8B018A21065DD0B72CA8BB1BE7BFC1167CEE286610A34A614C24EB721B4B0574B317DEF2CD3CB3D0810A4AE3A7C4491D319E578126321
	|
	|	徽标工具 1.1
	|
	|	项目by	Flashback-GTA
	|	翻译by	哑光
	|	原作者视频教程：https://www.youtube.com/watch?v=bGI88KDaKmY
	|
	|   这个工具使用Javascript到SVG渐变重新创建位图/光栅图像。
	|   以这种格式存储图像的效率都会低的离谱，例如，一个90kb的
	|   的图像(png)在转换后(svg)可以达到接近15MB的大小! 
	|   Rockstar编辑器可以接受几乎刚好超过1MB但小于1.01MB的图像。
	|   所以这个工具不能用于所有的图像，你可能需要进行调整（调整大小/降低更多的色彩）
	|   然后获得更小的容量上传到Rockstar徽标里
	|	
	|   这是一个有趣的项目，在Covid-19 封锁期间，主要是随心所欲地做出来的。
	|   我重新写了一些部分，并对其进行了整理。
	|   但它仍然很乱，而且有很多想法我都没有写进去。
	|   （比如检测源图像中的实际位图）。
	|   另外需要注意，这个方法可能会被R星所修补。
	|   所以它不值得实现更多的功能。
	|   如果R星修复了它，我也会去更新它。
	|
	|   当前页面由哑光维护 翻译 哑光
	|
	|
-->
<!doctype html>
<html lang="zh">
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<title>自定义帮会徽章 - 哑光</title><meta name="author" content="Flashback-GTA">
	<!-- CSS STYLES START -->
	<style>
		html,body,div,span,object,iframe,h1,h2,h3,h4,h5,h6,p,blockquote,pre,abbr,address,cite,code,del,dfn,em,img,ins,kbd,q,samp,small,strong,sub,sup,var,b,i,dl,dt,dd,ol,ul,li,fieldset,form,label,legend,table,caption,tbody,tfoot,thead,tr,th,td,article,aside,canvas,details,figcaption,figure,footer,header,hgroup,menu,nav,section,summary,time,mark,audio,video{margin:0;padding:0;border:0;outline:0;font-size:100%;vertical-align:baseline;background:transparent}body{line-height:1}article,aside,details,figcaption,figure,footer,header,hgroup,menu,nav,section{display:block}nav ul{list-style:none}blockquote,q{quotes:none}blockquote:before,blockquote:after,q:before,q:after{content:'';content:none}a{margin:0;padding:0;font-size:100%;vertical-align:baseline;background:transparent}ins{background-color:#ff9;color:#000;text-decoration:none}mark{background-color:#ff9;color:#000;font-style:italic;font-weight:700}del{text-decoration:line-through}abbr[title],dfn[title]{border-bottom:1px dotted;cursor:help}table{border-collapse:collapse;border-spacing:0}hr{display:block;height:1px;border:0;border-top:1px solid #ccc;margin:1em 0;padding:0}input,select{vertical-align:middle}
		html, body {
			margin:0px;
			height:100%;
			background-color: #242424;
			text-rendering: optimizeLegibility;
			-webkit-font-smoothing: antialiased;
			font-family: "Source Sans Pro", Helvetica, sans-serif;
			font-weight: 700;
		}
		fieldset {
			border: 2px solid #46494E;
			padding: 8px;    
			border-radius: 8px;
			max-width: 512px;
			margin: 16px auto;
		}
		legend {
			color: #777672;
			cursor: default;
			font-size: medium;
			font-weight: 600;
			padding: 0 8px;
			text-align: left;
			text-shadow: 1px 1px 2px #242424;
		}
		/*	https://stackoverflow.com/questions/23688025/making-a-simple-tooltip-with-only-html-and-css	*/
		.info:hover .tooltip {
			display: block;
		}
		.info {
			cursor: help;
			font-size: large;
			font-weight: 400;
			margin-right: 4px;
		}
		.tooltip {
			animation: fade 0.5s;
			display: none;
			color: #FFF;
			margin-left: 8px;
			margin-top: 8px;
			position: absolute;
			z-index: 1000;
			background-color: #292929;
			border: 2px solid #46494E;
			border-radius: 8px;
			font-size: small;
			line-height: 1.4;
			max-width: 512px;
			padding: 12px;
			box-shadow: 4px 4px 4px 2px rgb(0 0 0 / 0.2);
		}
		@keyframes fade {0% {opacity: 0;}100% {opacity: 1;}}
		@-moz-keyframes fade {0% {opacity: 0;}100% {opacity: 1;}}
		@-webkit-keyframes fade {0% {opacity: 0;}100% {opacity: 1;}}
		@-o-keyframes fade {0% {opacity: 0;}100% {opacity: 1;}}
		@-ms-keyframes fade {0% {opacity: 0;}100% {opacity: 1;}}
		.container {
			width: 100%;
			height: 100%;
		}
		.input {
		    line-height: normal;
			outline: none;
			font-weight: 700;
			padding: 4px 8px;
			width: auto;
			display: inline-block;
			margin-right: 4px;
			text-align: center;
			margin-bottom: 8px;
			font-size: 75%;
			-webkit-font-smoothing: antialiased;
			border-radius: 4px;
			white-space: nowrap;
			cursor: pointer;
			transition: all 0.6s ease-in;
			text-transform: uppercase;
			text-decoration: none;
			color: #FFF;
			background: #1183CF;
		}
		/*	Button adapted from: https://codepen.io/RafaelDeJongh/pen/pNEZgO ----------------- https://codepen.io/vatosteve/pen/pogdy */
		button {
			border-radius: 8px;
			outline: 0; 
			font-size: 18px;
			font-weight: bold;
			color: #FFF;
			background: #000;
			border:none;
			padding: 16px 24px;
			padding-top: 10px;
			margin: 8px 0px;
			transition: all .05s ease-out;
			box-shadow: inset 0 -6px 0 0 rgba(0,0,0,.2), 1px 1px 0 0 #292929, 2px 2px 0 0 #292929, 3px 3px 0 0 #292929, 4px 4px 0 0 #292929, 5px 5px 0 0 #292929, 6px 6px 0 0 #292929;
		}
		button:hover:enabled {
			color: #444;
		}
		button:active:enabled {
			color: #222;
			box-shadow: inset 0 -4px 0 0 rgba(0,0,0,.2), 1px 1px 0 0 #292929, 2px 2px 0 0 #292929, 3px 3px 0 0 #292929, 4px 4px 0 0 #292929;
		}
		.clr-inactive {	background-color: #CCCCCC;	}
		.clr-primary {	background-color: #428BCA;	}
		.clr-success {	background-color: #5CB85C;	}
		.clr-info {	background-color: #5bc0de;	}
		.clr-warning {	background-color: #f0ad4e;	}
		.clr-danger {	background-color: #d9534f;	}
		.btn-wide {
			width: 100%;
			max-width: 512px;
		}
		.row {
			height: 100%;
		}
		.row:after {
			content: "";
			display: table;
			clear: both;
		}
		.column {
			float: left;
			min-height: 100%;
		}
		.left, .right {
			width: 20%;
			background-color: #333;
			box-shadow: 0 0 8px 8px rgb(0 0 0 / 0.2);
		}
		.middle {
			width: 60%;
		}
		.column-content {
			padding: 16px;
			text-align: center;
			height: auto;
		}
		.div-info {
			border-radius: 8px;
			padding: 8px 0px;
			margin: 16px auto;
			max-width: 512px;
		}
		.div-status {
			padding-bottom: 8px;
  			align-items: center;
  			justify-content: center;
		}
		.status-text {
			text-align: center;
			font-size: small;
			font-weight: 600;
			color: #FFF;
		}
		.textarea {
			display: block;
			width: 100%;
			height: 512px;
			padding: 0px;
			border: 0px;
			margin: 8px 0px;
			overflow-y: scroll;
			resize: both;
			min-height: 40px;
			line-height: 20px;
			max-width: 512px;
		}
		.div-svg-container {
			margin: 8px;
			min-height: 512px;
			position: relative;
		}
		.div-svg-image {
			position: absolute;
			margin-left: auto;
			margin-right: auto;
			left: 0;
			right: 0;
			text-align: center;
		}
		.div-image-container {
			position: relative;
			width: 64px;
			height: 64px;
			background-color: #242424;
			background-position: center;
			background-size: cover;
			margin: 16px auto;
			text-align: center;
		}
		.div-image-container img {
			position: absolute;
			left: 0;
			top: 0;
			right: 0;
			bottom: 0;
			display: block;
			margin: auto;
			max-width: 64px;
			max-height: 64px;
			text-align: center;
		}
		.div-support-link {
			font-size: small;
			color: #FFF;
			margin: auto;
			padding: 8px;
			text-align: center;
			text-shadow: 2px 2px 2px #242424;
			max-width: 512px;
		}
		.div-support-link a {
			color: #FFF;
			text-decoration: none;
		}
		.div-support-link a:hover {
			text-decoration: underline;
		}
		.div-mobile {
			display: none;
			font-size: small;
			color: #FFF;
			padding: 8px;
			text-align: left;
			max-width: 512px;
		}
		/* https://codepen.io/avstorm/pen/eYNLwVd */
		.l-radio {
			padding: 6px;
			border-radius: 50px;
			display: inline-flex;
			cursor: pointer;
			transition: background 0.3s ease;
			margin: 6px 0;
			-webkit-tap-highlight-color: transparent;
		}
		.l-radio:hover, .l-radio:focus-within {
			background: rgba(159, 159, 159, 0.1);
		}
		.l-radio input {
			vertical-align: middle;
			width: 20px;
			height: 20px;
			border-radius: 10px;
			background: none;
			border: 0;
			box-shadow: inset 0 0 0 4px #CCCCCC;
			appearance: none;
			padding: 0;
			margin: 0;
			transition: box-shadow 150ms cubic-bezier(0.95, 0.15, 0.5, 1.25);
			pointer-events: none;
		}
		.l-radio input:focus {
			outline: none;
		}
		.l-radio input:checked {
			box-shadow: inset 0 0 0 16px #5CB85C;
		}
		.l-radio span {
			vertical-align: middle;
			display: inline-block;
			line-height: 20px;
			padding: 0 8px;
			font-weight: 500;
			color: #FFF;
		}
		/* Mobile devices and smaller screens */
		@media screen and (max-width: 1110px) {
			.column, .container {
				width: 100%;
				height: auto;
			}
			.textarea {
				margin-left: auto;
				margin-right: auto;
			}
			.div-mobile {
				display: block;
			}
		}
	</style>
	<!-- CSS STYLES END -->
	<!-- HELPER FUNCTIONS START -->
	<script>
		//	Kuzey Beytar @ https://stackoverflow.com/questions/22868475/how-can-i-trim-the-length-of-a-filename-in-javascript-but-keep-the-extension
		function fileTrunc(fileName) {
			if (fileName.length > 20)
				return fileName.substr(0, 10) + '[..]' + fileName.substr(-10);
			return fileName;
		}

		//	Elias Zamaria @ https://stackoverflow.com/questions/2901102/how-to-print-a-number-with-commas-as-thousands-separators-in-javascript
		function numberWithCommas(x) {
			return x.toString().replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ',');
		}
		
		//	Round float values to the given decimal place.
		function floatRound(value, decimals = 5) {
			var _n = parseFloat(value).toFixed(decimals);
			return Number(_n);
		}

		//	Tim Down @ https://stackoverflow.com/questions/5623838/rgb-to-hex-and-hex-to-rgb
		function componentToHex(c) {
			var _hex = c.toString(16);
			return _hex.length == 1 ? '0' + _hex : _hex;
		}
		function rgbToHex(r, g, b) {
			return '#' + componentToHex(r) + componentToHex(g) + componentToHex(b);
		}

		//	discuss at: https://locutus.io/php/base_convert/
		//	original by: Philippe Baumann
		//	improved by: Rafał Kukawski (https://blog.kukawski.pl)
		//	example 1: base_convert('A37334', 16, 2)
		//	returns 1: '101000110111001100110100'
		function base_convert (number, frombase, tobase) {
			return parseInt(number + '', frombase | 0).toString(tobase | 0)
		}

		//	Adapted from: https://stackoverflow.com/questions/25607844/css-hex-to-shorthand-hex-conversion
		function hexToShort(hex) {
			if (hex[0] == '#')
				hex = hex.substr(1, 6);
			
			if (hex.length == 3)
				return hex;
			
			if (hex.length != 6)
				return '#FFF';
			
			var _final = '';
			
			var _segments = [];
			_segments[0] = hex.substr(0, 2);
			_segments[1] = hex.substr(2, 2);
			_segments[2] = hex.substr(4, 2);
			
			for (const s of _segments)
			{
				_dec = base_convert(s, 16, 10);
				_remainder = _dec % 17;
				_newa = (_dec % 17 > 7) ? 17 + (_dec - _remainder) : _dec - _remainder;
				hex = base_convert(_newa, 10, 16);
				_final += hex[0];
			}
			return '#' + _final;
		}

		//	Array comparison.
		function arraysEqual(a, b) {
			return JSON.stringify(a) == JSON.stringify(b);
		}
		
		//	https://www.w3schools.com/howto/howto_js_copy_clipboard.asp
		function copyToClipboard(textArea, alert = false) {
			var _copyText = document.getElementById(textArea);
			var _textLength = _copyText.value.length;
			_copyText.select();
			_copyText.setSelectionRange(0, _textLength);
			navigator.clipboard.writeText(_copyText.value);
			if (alert)
				alert('Copied!');
		}
		
		/*	Jason J. Nathan @ https://stackoverflow.com/questions/3971841/how-to-resize-images-proportionally-keeping-the-aspect-ratio
		 *	Conserve aspect ratio of the original region. Useful when shrinking/enlarging
		 *	images to fit into a certain area.
		 *	@param {Number} srcWidth width of source image
		 *	@param {Number} srcHeight height of source image
		 *	@param {Number} maxWidth maximum available width
		 *	@param {Number} maxHeight maximum available height
		 *	@return {Object} { width, height }
		 */
		//	Added offsets for square image centering.
		function calculateAspectRatioFit(srcWidth, srcHeight, maxWidth, maxHeight) {
			var ratio = Math.min(maxWidth / srcWidth, maxHeight / srcHeight);
			return { width: srcWidth*ratio, height: srcHeight*ratio,
				offsetX: (maxWidth - srcWidth*ratio) / 2, offsetY: (maxHeight - srcHeight*ratio) / 2};
		}
		
		//	CSS helper functions.
		function setClass(element, cssClass) {
			document.getElementById(element).className = cssClass;
		}
		function enableElement(element) {
			document.getElementById(element).disabled = false;
		}
		function disableElement(element) {
			document.getElementById(element).disabled = true;
		}
		
	</script>
	<!-- HELPER FUNCTIONS END -->
	<!-- SVG OBJECT CLASS START -->
	<script>
		//	Adapted from: Gumape & Tobias Tengler @ https://stackoverflow.com/questions/3492322/javascript-createelementns-and-svg
		class svgObject
		{
			xmlns;
			canvasWidth;
			canvasHeight;
			svgElement;
			svgDefs;
			paths;
			shapePath;

			//	Create the SVG element with the standard attributes.
			constructor(shapePath, canvasWidth = 512, canvasHeight = 512)
			{
				this.xmlns = 'http://www.w3.org/2000/svg';
				this.canvasWidth = canvasWidth;
				this.canvasHeight = canvasHeight;
				this.svgElement = document.createElementNS(this.xmlns, 'svg');
				this.svgElement.setAttribute('xmlns', this.xmlns);
				this.svgElement.setAttribute('width', canvasHeight);
				this.svgElement.setAttribute('height', canvasHeight);
				this.svgElement.setAttribute('version', '1.1');
				this.svgDefs = document.createElementNS(this.xmlns, 'defs');
				this.paths = [];
				this.shapePath = shapePath;
			}
			
			//	Add the standard background rectangle.
			addBackground()
			{
				var _rect = document.createElementNS(this.xmlns, 'rect');
				_rect.setAttribute('x', 0);
				_rect.setAttribute('y', 0);
				_rect.setAttribute('width', this.canvasWidth);
				_rect.setAttribute('height', this.canvasHeight);
				_rect.setAttribute('rx', 0);
				_rect.setAttribute('ry', 0);
				_rect.setAttribute('fill', 'none');
				_rect.setAttribute('stroke', '#a1a1a1');
				_rect.setAttribute('fill-opacity', 1);
				_rect.setAttribute('stroke-opacity', 0);
				_rect.setAttribute('stroke-width', 0);
				_rect.setAttribute('stroke-miterlimit', 10);
				this.svgElement.appendChild(_rect);
			}

			//	Add a solid layer with only one colour.
			addSolidPath(x, y, scaleX, scaleY, fill, opacity)
			{
				var _path = document.createElementNS(this.xmlns, 'path');
				_path.setAttribute('fill', fill);
				var _alpha = floatRound((1 / 255) * opacity, 3);
				//	The alpha value only needs to be set if it is less than 1.
				if (_alpha < 1)
					_path.setAttribute('fill-opacity', _alpha);
				_path.setAttribute('d', this.shapePath);
				_path.setAttribute('transform', 'matrix('+ scaleX +',0,0,'+ scaleY +','+ x +','+ y +')');
				this.paths.push(_path);
			}
			
			//	Add a gradient layer with the given gradient.
			addGradientPath(x, y, scaleX, scaleY, fill)
			{
				var _path = document.createElementNS(this.xmlns, 'path');
				_path.setAttribute('fill', fill);
				_path.setAttribute('d', this.shapePath);
				_path.setAttribute('transform', 'matrix('+ scaleX +',0,0,'+ scaleY +','+ x +','+ y +')');
				this.paths.push(_path);
			}
			
			//	Add a new gradient definition to the SVG.
			addGradient(id, stops, isRows)
			{
				var _gradient = document.createElementNS(this.xmlns, 'linearGradient');
				_gradient.setAttribute('id', id);
				
				//	Gradient orientation (horizontal/vertical).
				if (isRows)
				{
					//	Horizontal gradient is the default so these attributes can be omitted.
					//_gradient.setAttribute('x2', 1);
					//_gradient.setAttribute('y2', 0);
				}
				else
				{
					_gradient.setAttribute('x2', 0);
					_gradient.setAttribute('y2', 1);
				}
				
				//	Add the stops to the gradient.
				for (const _stop of stops)
				{
					var _s = document.createElementNS(this.xmlns, 'stop');
					
					_s.setAttribute('offset', _stop[0]);
					_s.setAttribute('stop-color', _stop[1]);

					var _alpha = floatRound((1 / 255) * _stop[2], 3);
					
					//	The alpha value only needs to be set if it is less than 1.
					if (_alpha < 1)
						_s.setAttribute('stop-opacity', _alpha);

					_gradient.appendChild(_s);
				}
				
				this.svgDefs.appendChild(_gradient);
			}
			
			//	Construct the final SVG.
			finalise()
			{
				this.svgElement.appendChild(this.svgDefs);		//:	Gradient definitions.
				this.addBackground();							//:	Default background.
				for (const _path of this.paths)					//:	Paths for each slice/layer.
				{
					this.svgElement.appendChild(_path);
				}
			}

			// Return the full SVG element.
			getSVG()
			{
				return this.svgElement;
			}
		}
	</script>
	<!-- SVG OBJECT CLASS END -->
	<!-- EMBLEM HELPER CLASS START -->
	<script>
		//	A slice is a row or column of the emblem SVG.
		class emblemSlice
		{
			constructor(id, isVisible, stops, span = 1, isSpanned = false, spannedBy = null)
			{
				this.id = id;					//: The slice ID.
				this.isVisible = isVisible;		//: Is any part of the slice visible?
				this.stops = stops;				//: The stops in the slice (the points where colours start/stop)
				this.span = span;				//: The amount of slices that this slice spans.
				this.isSpanned = isSpanned;		//: Is this slice spanned by a previous slice?
				this.spannedBy = spannedBy;		//: The slice that spans this slice, if this slice is spanned.
			}
		}
		
		//	The main Emblem Helper object class.
		class emblemHelper
		{
			svg;						//: The SVG object that will be created from the source image.
			layers;						//: The layers that will be uploaded to the editor.
			indexCount;					//: Index for layer count.

			slug = 'rectangles/21';		//: This slug was chosen as it is the smallest rectangle in size (in bytes).
			slugName = '21';			//: The name of the slug in the editor.
			slugWidth = 66.47;			//: The width of the slug in the editor.
			slugHeight = 300;			//: The height of the slug in the editor.
			slugPath = 'M0,0H66.437V300H0V148.125C0,148.125,0.04,147.744,0,147.625C-0.063,147.437,0,147.062,0,147.062V0Z';		//:	The slug path from the editor.

			decimalPrecision;			//: The number of decimal places to round floats to.
			lowColourMode;				//: Low colour mode reduces image quality but also reduces the request size.
			smoothing;					//:	Turn smoothing on or off for image resizing.

			sourceImage;				//: The image uploaded by the user.
			sourceWidth;				//: The width of the uploaded image.
			sourceHeight;				//: The height of the uploaded image.

			canvas;						//: Used for drawing the image in the background.
			context;					//: Used for drawing the image in the background.

			emblemWidth = 512;			//: Standard emblem width.
			emblemHeight = 512;			//: Standard emblem height.

			sliceWidth;					//: The initial slice width, prior to any scaling/spanning.
			sliceHeight;				//: The initial slice height, prior to any scaling/spanning.
			gradStopSize;				//: The base distance between each gradient stop point (the size of the simulated pixels).

			scaleX;						//: The horizontal scaling to be applied to the slug/layer.
			scaleY;						//: The vertical scaling to be applied to the slug/layer.

			layerLength;				//:	The size of the JSON layer data.
			svgLength;					//: The size of the SVG element.
			encodedLength;				//:	The total size when encoded.
			maxEncodedLength;			//:	The maximum request size that will be accepted by the server.

			constructor()
			{
				this.reset();
				this.decimalPrecision = 5;
				this.lowColourMode = false;
				this.smoothing = false;
				this.maxEncodedLength = 128 * 10000;
			}

			reset()
			{
				this.svg = new svgObject(this.slugPath);		//:	Create a new SVG.
				this.indexCount = 0;							//:	Reset the index count for the layers.
				this.layers = [];								//:	Create the array to store the layers.
				//	Add the default background layer (with transparent fix).
				this.layers.push({"id":"background","name":"Background","type":"square","y":0,"x":0,"scaleY":100,"scaleX":100,"invertedY":false,"invertedX":false,"rotation":0,"opacity":100,"index":0,"color":"#transparent","isFilled":true,"internal":true,"locked":false,"tBold":false,"tItalic":false,"fontFamily":null,"borderColor":"#a1a1a1","borderSize":0,"gradientStyle":"Fill","slug":"rectangles/square","width":512,"height":512});
			}

			//	Set the number of decimals to round SVG elements to.
			setAccuracy(i)
			{
				this.decimalPrecision = parseInt(i);
			}

			//	Enable or disable image smoothing.
			useSmoothing(mode)
			{
				this.smoothing = mode;
			}

			//	Enable or disable low colour mode.
			useLowColour(mode)
			{
				this.lowColourMode = mode;
			}

			//	Process the image and create the emblem.
			createEmblem(imageSrc, isRows = true, sourceCanvasSize = 256)
			{
				this.reset();

				var _sizeX = sourceCanvasSize, _sizeY = sourceCanvasSize;
				var _offsetX = 0, _offsetY = 0;

				//	Import the uploaded image.
				this.sourceImage = new Image();
				this.sourceImage.src = imageSrc;
				
				//	Calculate the size and offsets.
				var _sizes = calculateAspectRatioFit(this.sourceImage.width, this.sourceImage.height, sourceCanvasSize, sourceCanvasSize);
				_offsetX = _sizes.offsetX;
				_offsetY = _sizes.offsetY;
				_sizeX = _sizes.width;
				_sizeY = _sizes.height;

				this.sourceWidth = sourceCanvasSize;
				this.sourceHeight =  sourceCanvasSize;
				
				//	Create a canvas for drawing the image.
				this.canvas = document.createElement('canvas');
				this.canvas.width = this.sourceWidth;
				this.canvas.height = this.sourceHeight;
				this.context = this.canvas.getContext('2d');
				
				//	Image resize smoothing.
				if (!this.smoothing)
				{
					this.context.webkitImageSmoothingEnabled = false;
					this.context.mozImageSmoothingEnabled = false;
					this.context.imageSmoothingEnabled = false;
				}				

				//	Draw the image to the canvas.
				this.context.drawImage(this.sourceImage, _offsetX, _offsetY, _sizeX, _sizeY);

				//	Set the scale and sizing depending on the orientation of the emblem slices.
				if (isRows)
				{
					this.sliceWidth = this.emblemWidth;
					this.sliceHeight = this.emblemHeight / this.sourceHeight;
					this.gradStopSize = 1 / this.sourceWidth;
					this.scaleX = this.emblemWidth / this.slugWidth;
					this.scaleY = (this.emblemHeight / this.sourceHeight ) / this.slugHeight;

					this.createEmblemRows();
				}
				else
				{
					this.sliceWidth = this.emblemWidth / this.sourceWidth;
					this.sliceHeight = this.emblemHeight;
					this.gradStopSize = 1 / this.sourceHeight;
					this.scaleX = (this.emblemWidth / this.sourceWidth) / this.slugWidth;
					this.scaleY = this.emblemHeight / this.slugHeight;

					this.createEmblemColumns();
				}

				this.svg.finalise();

				//	Calculate the request size.
				var _svgElement = this.svg.getSVG().outerHTML;
				this.svgLength = _svgElement.length;
				this.layerLength = JSON.stringify(this.layers).length;
				this.encodedLength = Math.ceil((this.svgLength + this.layerLength) / 3) * 4;	//:	There is a bug which sometimes makes this calculation 4 bytes off from the true size.
			}

			//	Find the pixel value at the x,y coordinates of the source image.
			getPixel(x, y)
			{
				return this.context.getImageData(x, y, 1, 1).data;
			}
			
			//	Create a unique path ID.
			createPathID(i)
			{
				//	Note: Can this be shorter?
				var _dateTime = new Date().getTime();
				var _id = _dateTime + '' + i;
				return "s" + _id;
			}
			
			//	Create a new layer.
			createNewLayer(x, y, index, fill, isRows)
			{
				if (isRows)
				{
					var _y = (y * this.sliceHeight) - ((this.slugHeight - this.sliceHeight) / 2);
					var _x = x - ((this.slugWidth - this.sliceWidth) / 2);
				}
				else
				{
					var _y = y - ((this.slugHeight - this.sliceHeight) / 2);
					var _x = (x * this.sliceWidth) - ((this.slugWidth - this.sliceWidth) / 2);
				}

				_y = floatRound(_y, this.decimalPrecision);
				_x = floatRound(_x, this.decimalPrecision);

				var _scaleY = floatRound(this.scaleY * 100, this.decimalPrecision);
				var _scaleX = floatRound(this.scaleX * 100, this.decimalPrecision);

				//	Minimum layer scale in the crew emblem editor appears to be 1.
				if (_scaleX < 1)
					_scaleX = 1;
				if (_scaleY < 1)
					_scaleY = 1;

				var _id = this.createPathID(index);

				let _layer = {
					id: _id,
					name: this.slugName,
					type: 'path',
					y: _y,
					x: _x,
					scaleY: _scaleY,
					scaleX: _scaleX,
					invertedY: 0,	//:	false	(save some bytes)
					invertedX: 0,	//:	false
					rotation: 0,
					opacity: 100,
					index: index,
					color: fill,
					isFilled: 1,	//:	true
					internal: 0,	//:	false
					locked: 0,		//:	false
					tBold: 0,		//:	false
					tItalic: 0,		//:	false
					fontFamily: null,
					borderColor: '#0',	//:	#0 is accepted as a colour but is the same as 'none'.
					borderSize: 0,
					gradientStyle: 'Fill',
					slug: this.slug,
					width: this.slugWidth,
					height: this.slugHeight
				};
				
				return _layer;
			}
						
			//	Loop through the image and create slices.
			createEmblemColumns()
			{
				var _slices = [];
				//var _nextStopOffset = 1 / Math.pow(10, this.decimalPrecision + 1);
				
				for (let x = 0; x < this.sourceWidth; x++)
				{
					var _gradientId = x;				//: Short gradient name.
					var _stops = [];

					var _hasVisiblePixels = false;		//: Track if any part of the slice is visible.

					for (let y = 0; y < this.sourceHeight; y++)
					{
						var _color = this.getPixel(x, y);
						var _colorNext = this.getPixel(x, y + 1);
						var _colorPrev = this.getPixel(x, y - 1);

						var _colorHex = rgbToHex(_color[0],_color[1],_color[2]);
						var _colorNextHex = rgbToHex(_colorNext[0],_colorNext[1],_colorNext[2]);
						var _colorPrevHex = rgbToHex(_colorPrev[0],_colorPrev[1],_colorPrev[2]);

						if (this.lowColourMode)
						{
							_colorHex = hexToShort(_colorHex);
							_colorNextHex = hexToShort(_colorNextHex);
							_colorPrevHex = hexToShort(_colorPrevHex);
						}

						//	Track if the current slice contains any visible pixels.
						if (_color[3] > 0 && _hasVisiblePixels == false)
							_hasVisiblePixels = true;
						
						//	Add a new gradient stop (end the current colour) if:
						//	a) The current pixel is different to the previous one.
						//		OR
						//	b) The current pixel is the same as the previous one but the alpha is different.
						if (((_colorHex != _colorPrevHex) || (_colorHex == _colorPrevHex && _color[3] != _colorPrev[3]) ) && y != 0 && y != this.sourceHeight)
						{
							var _offsetA = floatRound(((y - 1) * this.gradStopSize) + this.gradStopSize, this.decimalPrecision);
							//_offsetA += _nextStopOffset;	//: This doesn't appear to make any difference. Need to test more.
							//_offsetA = floatRound(_offsetA, this.decimalPrecision + 1);
							if (_offsetA > 1)
								_offsetA = 1;
							_stops.push([_offsetA, _colorHex, _color[3]]);
						}

						//	Add a new gradient stop (start the next colour) if:
						//	a) The current pixel is different to the next one.
						//		OR
						//	b) The current pixel is the same as the next one but the alpha is different.
						if ((_colorHex != _colorNextHex) || (_colorHex == _colorNextHex) && (_color[3] != _colorNext[3]))
						{
							var _offsetB = floatRound((y * this.gradStopSize) + this.gradStopSize, this.decimalPrecision);
							if (_offsetB > 1)
								_offsetB = 1;
							_stops.push([_offsetB, _colorHex, _color[3]]);
						}
					}
					
					//	If the current slice is the same as the previous one, extend the previous slice instead of creating a new one.
					if (x > 0 && x < this.sourceWidth && arraysEqual(_stops, _slices[x - 1].stops))
					{
						//	If the slice before this one was spanned by another, extend that one instead.
						if (_slices[x - 1].isSpanned)
						{
							var _originator = _slices[x - 1].spannedBy;
							_slices[x] = new emblemSlice(x, _hasVisiblePixels, _stops, 1, true, _originator);
							_slices[_originator].span += 1;
						}
						else
						{
							_slices[x] = new emblemSlice(x, _hasVisiblePixels, _stops, 1, true, x - 1);
							_slices[x - 1].span += 1;
						}
					}
					else
					{
						_slices[x] = new emblemSlice(x, _hasVisiblePixels, _stops, 1);
					}
					
				}

				//	Loop through all the slices and create SVG elements for the visible ones.
				for (const _slice of _slices)
				{
					if (_slice.isVisible && !_slice.isSpanned)
					{
						//	If a slice only has 1 colour/stop, it can be set as a standard fill instead of a gradient fill.
						if (_slice.stops.length == 1)
						{
							var _hex = _slice.stops[0][1];
							var _alpha = _slice.stops[0][2];
							this.svg.addSolidPath(_slice.id * this.sliceWidth, 0, floatRound(this.scaleX * _slice.span, this.decimalPrecision), floatRound(this.scaleY, this.decimalPrecision), _hex, _alpha);
						}
						else
						{
							this.svg.addGradient(_slice.id, _slice.stops, false);
							var _fill = 'url(#'+ _slice.id +')';
							this.svg.addGradientPath(_slice.id * this.sliceWidth, 0, floatRound(this.scaleX * _slice.span, this.decimalPrecision), floatRound(this.scaleY, this.decimalPrecision), _fill);
						}
						this.layers.push(this.createNewLayer(_slice.id, 0, this.indexCount++, '#000', false));	//:	#000 could be changed to '#0' or 'red' to save a few bytes.
					}
				}
			}

			createEmblemRows()
			{
				var _slices = [];
				for (let y = 0; y < this.sourceHeight; y++)
				{
					var _gradientId = y;
					var _stops = [];
					var _hasVisiblePixels = false;

					for (let x = 0; x < this.sourceWidth; x++)
					{
						var _color = this.getPixel(x, y);
						var _colorNext = this.getPixel(x + 1, y);
						var _colorPrev = this.getPixel(x - 1, y);
						var _colorHex = rgbToHex(_color[0],_color[1],_color[2]);
						var _colorNextHex = rgbToHex(_colorNext[0],_colorNext[1],_colorNext[2]);
						var _colorPrevHex = rgbToHex(_colorPrev[0],_colorPrev[1],_colorPrev[2]);

						if (this.lowColourMode)
						{
							_colorHex = hexToShort(_colorHex);
							_colorNextHex = hexToShort(_colorNextHex);
							_colorPrevHex = hexToShort(_colorPrevHex);
						}

						if (_color[3] > 0 && _hasVisiblePixels == false)
							_hasVisiblePixels = true;
						
						if (((_colorHex != _colorPrevHex) || (_colorHex == _colorPrevHex && _color[3] != _colorPrev[3]) ) && x != 0 && x != this.sourceWidth)
						{
							var _offsetA = floatRound(((x - 1) * this.gradStopSize) + this.gradStopSize, this.decimalPrecision);
							if (_offsetA > 1)
								_offsetA = 1;
							_stops.push([_offsetA, _colorHex, _color[3]]);
						}

						if ((_colorHex != _colorNextHex) || (_colorHex == _colorNextHex) && (_color[3] != _colorNext[3]))
						{
							var _offsetB = floatRound((x * this.gradStopSize) + this.gradStopSize, this.decimalPrecision);
							if (_offsetB > 1)
								_offsetB = 1;
							_stops.push([_offsetB, _colorHex, _color[3]]);
						}
					}

					if (y > 0 && y < this.sourceHeight && arraysEqual(_stops, _slices[y - 1].stops))
					{
						if (_slices[y - 1].isSpanned)
						{
							var _originator = _slices[y - 1].spannedBy;
							_slices[y] = new emblemSlice(y, _hasVisiblePixels, _stops, 1, true, _originator);
							_slices[_originator].span += 1;
						}
						else
						{
							_slices[y] = new emblemSlice(y, _hasVisiblePixels, _stops, 1, true, y - 1);
							_slices[y - 1].span += 1;
						}
					}
					else
					{
						_slices[y] = new emblemSlice(y, _hasVisiblePixels, _stops, 1);
					}
				}

				for (const _slice of _slices)
				{
					if (_slice.isVisible && !_slice.isSpanned)
					{
						if (_slice.stops.length == 1)
						{
							var _hex = _slice.stops[0][1];
							var _alpha = _slice.stops[0][2];
							this.svg.addSolidPath(0, _slice.id * this.sliceHeight, floatRound(this.scaleX, this.decimalPrecision), floatRound(this.scaleY * _slice.span, this.decimalPrecision), _hex, _alpha);
						}
						else
						{
							this.svg.addGradient(_slice.id, _slice.stops, true);
							var _fill = 'url(#'+ _slice.id +')';
							this.svg.addGradientPath(0, _slice.id * this.sliceHeight, floatRound(this.scaleX, this.decimalPrecision), floatRound(this.scaleY * _slice.span, this.decimalPrecision), _fill);
						}
						this.layers.push(this.createNewLayer(0, _slice.id, this.indexCount++, '#000', true));
					}
				}
			}

			//	Create the code to be pasted into the browser console.
			getConsoleCode()
			{
				//	Base64 encode the SVG and layer data.
				var _svgData = btoa(this.svg.getSVG().outerHTML);
				var _layerData = btoa(JSON.stringify(this.layers));
				//	Create the request.
				var _consoleCode = '// Generated with EmblemHelper v1.1 \n\n';
				_consoleCode += 'var svgData = "' + _svgData + '";\n\n';
				_consoleCode += 'var layerData = "' + _layerData + '";\n\n';
				_consoleCode += 'var request = new XMLHttpRequest;request.open("POST","/emblems/save",!0),request.onreadystatechange=function(){if(request.readyState==XMLHttpRequest.DONE){var a=JSON.parse(request.responseText);200==a.Status?window.location.href="https://socialclub.rockstargames.com/emblems/edit/"+a.EmblemId:a.Message?alert(a.Message):alert(a.Error.Message)}},request.setRequestHeader("Content-Type","application/json"),request.setRequestHeader("__RequestVerificationToken",document.getElementsByName("__RequestVerificationToken")[0].value),request.setRequestHeader("X-Requested-With","XMLHttpRequest"),request.send(JSON.stringify({"crewId":"0","emblemId":"","parentId":"","svgData":svgData,"layerData": layerData,"hash":document.getElementById("editorField-hash").value}));';
				
				return _consoleCode;
			}

		}		
	</script>
	<!-- EMBLEM HELPER CLASS END -->
</head>
<body>
	<div class="container">
		<div class="row">
			<!-- LEFT COLUMN START -->
			<div class="column left">
				<div class="column-content">
					<button id="eh-btnImportImage" type="button" class="clr-primary btn-wide">导入图像</button>
					<input type="file" id="eh-fileSelector" style="display: none;">
					<div>
						<fieldset style="border-color: #242424;">
							<div class="div-image-container">
								<img id="eh-imgSource"/>
							</div>
							<div class="div-status">
								<span class="status-text" id="eh-lblStatus">没有图像导入</span>
							</div>
						</fieldset>
					</div>
					<div>
						<fieldset>
							<legend>
								<span class="info">🛈<span class="tooltip">
									决定源图像的分辨率。当生成图像时，初始尺寸将被自动选择。<br>
									<br>
									64 &nbsp; = &nbsp; 64 x 64 分辨率<br>
									128 &nbsp; = &nbsp; 128 x 128 分辨率<br>
									150 &nbsp; = &nbsp; 150 x 150 分辨率<br>
									160 &nbsp; = &nbsp; 160 x 160 分辨率<br>
									175 &nbsp; = &nbsp; 175 x 175 分辨率<br>
									200 &nbsp; = &nbsp; 200 x 200 分辨率<br>
									256 &nbsp; = &nbsp; 256 x 256 分辨率<br>
									512 &nbsp; = &nbsp; 512 x 512 分辨率
								</span></span>
								调整原图大小
							</legend>
							<div>
								<label for="eh-opt64" class="l-radio">
									<input type="radio" id="eh-opt64" value="64" name="eh-optSourceSize" tabindex="1">
									<span>64</span>
								</label>
								<label for="eh-opt128" class="l-radio">
									<input type="radio" id="eh-opt128" value="128" name="eh-optSourceSize" tabindex="2" checked="checked">
									<span>128</span>
								</label>
								<label for="eh-opt150" class="l-radio">
									<input type="radio" id="eh-opt150" value="150" name="eh-optSourceSize" tabindex="3" checked="checked">
									<span>150</span>
								</label>
								<label for="eh-opt160" class="l-radio">
									<input type="radio" id="eh-opt160" value="160" name="eh-optSourceSize" tabindex="4" checked="checked">
									<span>160</span>
								</label>
								<label for="eh-opt175" class="l-radio">
									<input type="radio" id="eh-opt175" value="175" name="eh-optSourceSize" tabindex="5" checked="checked">
									<span>175</span>
								</label>
								<label for="eh-opt200" class="l-radio">
									<input type="radio" id="eh-opt200" value="200" name="eh-optSourceSize" tabindex="6" checked="checked">
									<span>200</span>
								</label>
								<label for="eh-opt256" class="l-radio">
									<input type="radio" id="eh-opt256" value="256" name="eh-optSourceSize" tabindex="7">
									<span>256</span>
								</label>
								<label for="eh-opt512" class="l-radio">
									<input type="radio" id="eh-opt512" value="512" name="eh-optSourceSize" tabindex="8">
									<span>512</span>
								</label>
							</div>
						</fieldset>
						<fieldset>
							<legend>
								<span class="info">🛈<span class="tooltip">
									在这里选择是否启用平滑图像<br>
									<br>
									启用 &nbsp; = &nbsp; 启用平滑图像 &nbsp; (抗锯齿)<br>
									停用 &nbsp; = &nbsp; 停用平滑图像/锯齿外观
								</span></span>
								调整图片平滑
							</legend>
							<div>
								<label for="eh-optOn" class="l-radio">
									<input type="radio" id="eh-optOn" value="low" name="eh-optSmoothing" tabindex="1" checked="checked">
									<span>启用</span>
								</label>
								<label for="eh-optOff" class="l-radio">
									<input type="radio" id="eh-optOff" value="full" name="eh-optSmoothing" tabindex="2">
									<span>停用</span>
								</label>
							</div>
						</fieldset>
						<fieldset>
							<legend>
								<span class="info">🛈<span class="tooltip">
									创建徽标时使用的颜色模式<br>
									<br>
									低色彩 &nbsp; = &nbsp; 减少部分色彩 &nbsp; (对服务器的负载很小)<br>
									完整色彩 &nbsp; = &nbsp; 源色彩 &nbsp; (这会让服务器负载更大)
								</span></span>
								调整颜色模式
							</legend>
							<div>
								<label for="eh-optLow" class="l-radio">
									<input type="radio" id="eh-optLow" value="low" name="eh-optColour" tabindex="1">
									<span>低色彩</span>
								</label>
								<label for="eh-optFull" class="l-radio">
									<input type="radio" id="eh-optFull" value="full" name="eh-optColour" tabindex="2" checked="checked">
									<span>完整色彩</span>
								</label>
							</div>
						</fieldset>
						<fieldset>
							<legend>
								<span class="info">🛈<span class="tooltip">
									确定徽章创建方法，根据源图片的视觉元素，每一行将导致性能较弱的服务器请求大小。<br>
									<br>
									自动 &nbsp; = &nbsp; 自动计算水平和垂直，并选择最小值 &nbsp; (速度会更慢)<br>
									行 &nbsp; = &nbsp; 水平<br>
									列 &nbsp; = &nbsp; 垂直<br>
									<br>
									“自动”选项每次都会找到最佳方法，但在某些服务器上处理较大的图片可能需要更长的时间。
								</span></span>
								SVG 创建方式
							</legend>
							<div>
								<label for="eh-optAuto" class="l-radio">
									<input type="radio" id="eh-optAuto" name="eh-optOrientation" tabindex="1" checked="checked">
									<span>自动</span>
								</label>
								<label for="eh-optRows" class="l-radio">
									<input type="radio" id="eh-optRows" name="eh-optOrientation" tabindex="2">
									<span>行</span>
								</label>
								<label for="eh-optColumns" class="l-radio">
									<input type="radio" id="eh-optColumns" name="eh-optOrientation" tabindex="3">
									<span>列</span>
								</label>
							</div>
						</fieldset>
						<fieldset>
							<legend>
								<span class="info">🛈<span class="tooltip">
									确定用于徽标 SVG 元素的小数位数。<br>
									<br>
									低 &nbsp; = &nbsp; 3 &nbsp; (推荐)<br>
									中 &nbsp; = &nbsp; 4<br>
									高 &nbsp; = &nbsp; 5<br>
									<br>
									仅当有涂装在载具上有明显撕裂（颜色之间有线条）的时候，才建议设置为中或高。
								</span></span>
								十进制精度
							</legend>
							<div>
								<label for="eh-opt3" class="l-radio">
									<input type="radio" id="eh-opt3" value="3" name="eh-optAccuracy" tabindex="3" checked="checked">
									<span>低</span>
								</label>
								<label for="eh-opt4" class="l-radio">
									<input type="radio" id="eh-opt4" value="4" name="eh-optAccuracy" tabindex="4">
									<span>中</span>
								</label>
								<label for="eh-opt5" class="l-radio">
									<input type="radio" id="eh-opt5" value="5" name="eh-optAccuracy" tabindex="5">
									<span>高</span>
								</label>
							</div>
						</fieldset>
						<button disabled id="eh-btnProcessImage" type="button" class="clr-inactive btn-wide">创建徽章</button>
					</div>
					
				</div>
			</div>
			<!-- LEFT COLUMN END -->
			<!-- MIDDLE COLUMN START -->
			<div class="column middle">
				<div class="column-content">
					<div class="div-svg-container">
						<div class="div-svg-image">
							<svg width="512" height="512">
								<defs>
									<pattern id="checkerDark" x="0" y="0" width="16" height="16" patternUnits="userSpaceOnUse">
										<rect x="0" y="0" width="8" height="8" fill="#46494E"></rect>
										<rect x="0" y="8" width="8" height="8" fill="#777672"></rect>
										<rect x="8" y="0" width="8" height="8" fill="#777672"></rect>
										<rect x="8" y="8" width="8" height="8" fill="#46494E"></rect>
									</pattern>
								</defs>
								<rect x="0" y="0" width="100%" height="100%" fill="url(#checkerDark)"></rect>
							</svg>
						</div>
						<div class="div-svg-image" id="eh-svgPreview"></div>
					</div>
					<div id="eh-divInfo" class="div-info clr-inactive">⠀</div>
					<div class="div-support-link">
						<a href="https://space.bilibili.com/286090636" target="_blank">获取帮助 点击这里私信！在制作之建议您将图片转换为矢量图</a>
					</div>
				</div>
			</div>
			<!-- MIDDLE COLUMN END -->
			<!-- RIGHT COLUMN START -->
			<div class="column right">
				<div class="column-content">
					<button disabled id="eh-btnCreateCode" type="button" class="clr-inactive btn-wide">生成代码</button>
					<textarea id="eh-txtConsoleCode" class="textarea"></textarea>
					<div class="div-info div-mobile">
						<strong>手机用户需要注意的事项</strong><br>
						某些手机将无法复制所有代码，您可能需要一部分一部分的去复制它。
					</div>
					<button disabled id="eh-btnCopyCode" type="button" class="clr-inactive btn-wide">复制代码</button>
				</div>
			</div>
			<!-- RIGHT COLUMN END -->
		</div>
	</div>
	<script>
		//	Adapated from:	https://web.dev/read-files/
		const STATUS = document.getElementById('eh-lblStatus');
		const OUTPUT = document.getElementById('eh-imgSource');
		if (window.FileList && window.File && window.FileReader)
		{
			document.getElementById('eh-fileSelector').addEventListener('change', event => {
				OUTPUT.src = '';
				STATUS.textContent = '没有图像导入';
				//	Disable the Create Emblem button.
				setClass('eh-btnProcessImage', 'btn-wide clr-inactive');
				disableElement('eh-btnProcessImage');
				const FILE = event.target.files[0];

				if (!FILE.type)
				{
					STATUS.textContent = '错误：你当前使用的浏览器似乎不支持Type属性 请更换浏览器';
					return;
				}

				if (!FILE.type.match('image.*'))
				{
					STATUS.textContent = '错误：无法识别这个图像格式 请重新选择'
					return;
				}

				const READER = new FileReader();
				READER.addEventListener('load', event => {
					OUTPUT.src = event.target.result;
					var _path = document.getElementById('eh-fileSelector').value;
					var _fName = _path.split("\\").pop();
					STATUS.textContent = fileTrunc(_fName);
					//	Enable the Create Emblem button.
					setClass('eh-btnProcessImage', 'btn-wide clr-primary');
					enableElement('eh-btnProcessImage');
				});

				//	Set the initial size for the imported image.
				OUTPUT.addEventListener('load', event => {
					var _w = OUTPUT.naturalWidth;
					var _h = OUTPUT.naturalHeight;

					if (_w >= 512 || _h >= 512)
					{
						document.getElementById('eh-opt512').checked = true;
					}
					else if (_w >= 256 || _h >= 256)
					{
						document.getElementById('eh-opt256').checked = true;
					}
					else if (_w >= 128 || _h >= 128)
					{
						document.getElementById('eh-opt128').checked = true;
					}
					else
					{
						document.getElementById('eh-opt64').checked = true;
					}
				});
				READER.readAsDataURL(FILE);
			}); 
		}

		//	Import Image button clicked : trigger the file select dialog.
		document.getElementById('eh-btnImportImage').onclick = function() {
			document.getElementById('eh-fileSelector').click();
		};

		//	Create Emblem button clicked : check the selected options and process the image.
		document.getElementById('eh-btnProcessImage').onclick = function()
		{
			//	Disable the Create Code button.
			setClass('eh-btnCreateCode', 'btn-wide clr-inactive');
			disableElement('eh-btnCreateCode');

			//	Disable the Copy Code button.
			setClass('eh-btnCopyCode', 'btn-wide clr-inactive');
			disableElement('eh-btnCopyCode');

			//	Clear the console code text area.
			document.getElementById('eh-txtConsoleCode').value = '';

			//	Create a new Emblem Helper object.
			EH = new emblemHelper();

			//	Get the selected source image canvas size.
			var _resize = 64, _optionResize = document.getElementsByName('eh-optSourceSize');
			for (i = 0; i < _optionResize.length; i++)
				if(_optionResize[i].checked)
					_resize = Number(_optionResize[i].value);
			
			//	Get the selected decimal precision.
			var _precision = 3, _optionAccuracy = document.getElementsByName('eh-optAccuracy');
			for (i = 0; i < _optionAccuracy.length; i++)
				if(_optionAccuracy[i].checked)
					_precision = Number(_optionAccuracy[i].value);
			
			//	Set the decimal precision in the Emblem Helper object.
			EH.setAccuracy(_precision);

			//	Get the selected smoothing mode (on/off).
			document.getElementById('eh-optOn').checked ? (EH.useSmoothing(true)) : (EH.useSmoothing(false));

			//	Get the selected colour mode (low/full).
			document.getElementById('eh-optLow').checked ? (EH.useLowColour(true)) : (EH.useLowColour(false));

			var _sourceImage = document.getElementById('eh-imgSource').src;
			
			//	Create the emblem using the selected method (auto/rows/columns).
			if (document.getElementById('eh-optRows').checked)
			{
				EH.createEmblem(_sourceImage, true, _resize);
			}
			else if (document.getElementById('eh-optColumns').checked)
			{
				EH.createEmblem(_sourceImage, false, _resize);
			}
			else if (document.getElementById('eh-optAuto').checked)
			{
				//	NOTE: Inefficient. Needs work.
				EH.createEmblem(_sourceImage, false, _resize);
				var _lengthColumns = EH.encodedLength;
				EH.createEmblem(_sourceImage, true, _resize);
				var _lengthRows = EH.encodedLength;
				if (_lengthColumns < _lengthRows)
					EH.createEmblem(_sourceImage, false, _resize);
			}

			//	Display the emblem SVG preview.
			document.getElementById('eh-svgPreview').innerHTML = '';
			document.getElementById('eh-svgPreview').appendChild(EH.svg.getSVG());

			//	Check the size of the resulting emblem.
			if (EH.encodedLength < EH.maxEncodedLength)
			{
				//	The eblem is small enough to upload.
				setClass('eh-divInfo', 'div-info clr-success');
				document.getElementById('eh-divInfo').innerHTML = '完成 该大小可以生成:&nbsp;&nbsp;&nbsp; ' + numberWithCommas(EH.encodedLength) + '&nbsp;&nbsp;/&nbsp;&nbsp;' + numberWithCommas(EH.maxEncodedLength);
				//	Enable the Create Code button
				setClass('eh-btnCreateCode', 'btn-wide clr-primary');
				enableElement('eh-btnCreateCode');
			}
			else
			{
				//	The emblem is too large to upload.
				setClass('eh-divInfo', 'div-info clr-danger');
				document.getElementById('eh-divInfo').innerHTML = '图像过大 无法生成:&nbsp;&nbsp;&nbsp; ' + numberWithCommas(EH.encodedLength) + '&nbsp;&nbsp;/&nbsp;&nbsp;' + numberWithCommas(EH.maxEncodedLength);
			}
			
		};

		//	Create Code button clicked : display the console code.
		document.getElementById('eh-btnCreateCode').onclick = function()
		{
			document.getElementById('eh-txtConsoleCode').value = EH.getConsoleCode();
			//	Enable the Copy Code button.
			setClass('eh-btnCopyCode', 'btn-wide clr-primary');
			enableElement('eh-btnCopyCode');
		};

		//	Copy Code button clicked : copy the code to the clipboard.
		document.getElementById('eh-btnCopyCode').onclick = function()
		{
			copyToClipboard('eh-txtConsoleCode');
			//	Notify the user that the code was copied.
			document.getElementById('eh-btnCopyCode').innerText = '已复制';
			setClass('eh-btnCopyCode', 'btn-wide clr-success');
			setTimeout(function () { document.getElementById('eh-btnCopyCode').innerText = '复制代码', setClass('eh-btnCopyCode', 'btn-wide clr-primary') }, 2000);
		};
	</script>
</body>
</html>