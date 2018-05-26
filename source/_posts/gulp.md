---
title: 'Gulp JavaScript构建系统'
date: 2018-05-24 11:19:24
tags:
---

标签（空格分隔）： C#

---

在此输入正文
## 1. 全局安装 gulp：
```
$ npm install --global gulp
```

## 2. 作为项目的开发依赖（devDependencies）安装：
```
$ npm install --save-dev gulp
```

## 3. 在项目根目录下创建一个名为 gulpfile.js 的文件：
```
var gulp = require('gulp');

gulp.task('default', function() {
  // 将你的默认的任务代码放在这
});
```

## 4. 运行 gulp：
```
$ gulp
```

## 5. 在Asp.net Core项目中添加gulp
```
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
    rimraf = require("rimraf"),
    concat = require("gulp-concat"),
    cssmin = require("gulp-cssmin"),
    uglify = require("gulp-uglify");

var webroot = "./wwwroot/";

var paths = {
    js: webroot + "js/**/*.js",
    minJs: webroot + "js/**/*.min.js",
    css: webroot + "css/**/*.css",
    minCss: webroot + "css/**/*.min.css",
    concatJsDest: webroot + "js/site.min.js",
    concatCssDest: webroot + "css/site.min.css"
};

gulp.task("clean:js", function (cb) {
    rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
    rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
    return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
    return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

### 6. 在Visual Studio 2015中运行
右键点击gulp文件，选择“任务运行程序资源管理器”，如下图：
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/gulp/1.png)