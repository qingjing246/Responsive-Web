##开发文档
###一、项目需求
	实现页面响应式，在设备宽度800像素以上,800-480像素以内,480像素以下进行自适应。

###二、项目文件夹设置
####1、最外部文件
	1.src文件夹：所有项目主要文件；
	2..editorconfig：来规范代码缩进等的风格；
	3..gitignore.txt：就是告诉Git哪些文件不需要添加到版本管理中;
	4.README.md：用于项目介绍，使用markdown编写；
####2、src内文件
	1.css文件夹：用于所有样式文件存放；
	2.img文件夹：用于所有图片文件存放；
	3.js文件夹：用于所有javascript文件存放;
	4.404.html：页面为能加载是显示的错误页面；
	5.humans.txt:开发人员介绍；
	6.index.html:主页；
	7.robots.txt:设置禁止爬虫抓取；
###三、编写规范
#####1.命名和样式规范
	大小单位使用rem,使用utf-8编码，其他无要求，尽量使用语义化并按照W3C规范；
####1.轮播广告
	使用jquery的owl.carousel.js插件，它很好的兼容各个浏览器和触屏操作

	<div class="owl-carousel  owl-theme">
         <div class="item">
			 <img src="img/BG1.jpg" alt="图片未加载"/>
	     </div>
	     <div class="item">
             <img src="img/BG2.jpg" alt="图片未加载"/>
	     </div>
         <div class="item">
              <img src="img/BG3.jpg" alt="图片未加载"/>
         </div>
     </div>
#####2.图片响应式
	使用picturefill.js实现图片响应式

	<picture>
         <source srcset="img/BG1l.jpg" media="(max-width:50em)">
         <img src="img/BG1.jpg" alt="图片未加载"/>
    </picture>

###四、打包发布
	使用gulp进行文件打包压缩
	var gulp=require('gulp');
var rev = require('gulp-rev');//版本更新
var revReplace = require('gulp-rev-replace'); //引用更新
var useref = require('gulp-useref'); //所有CSS,JS文件合并
var filter = require('gulp-filter');//压缩CSS,JS文件
var uglify = require('gulp-uglify');//压缩JS代码
var csso = require('gulp-csso');//压缩CSS代码


gulp.task('default',function(){
    var jsFile = filter('**/*.js',{restore:true});
    var cssFile = filter('**/*.css',{restore:true});
    var indexHtmlFilter = filter(['**/*','!**/index.html'],{restore:true});

    return gulp.src('src/index.html')
        .pipe(useref())//获取页面标识的所有JS.CSS文件
        .pipe(jsFile)//所有JS文件
        .pipe(uglify())//压缩所有JS代码
        .pipe(jsFile.restore)//把压缩后的代码返回文件流中
        .pipe(cssFile)//获取CSS文件
        .pipe(csso())//压缩CSS代码
        .pipe(cssFile.restore)//返回CSS文件
        .pipe(indexHtmlFilter)//排除HTML文件
        .pipe(rev())//给JS.CSS文件打版本号
        .pipe(indexHtmlFilter.restore)//返回HTML文件
        .pipe(revReplace())//文件引用更新
        .pipe(gulp.dest('dist'));//任务结束，文件返回到dist文件下
});

