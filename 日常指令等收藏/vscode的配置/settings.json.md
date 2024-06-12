{

// vscode默认启用了根据文件类型自动设置tabsize的选项

"editor.detectIndentation": false,

// 重新设定tabsize

"editor.tabSize": 2,

// #每次保存的时候自动格式化

"editor.formatOnSave": true,

// #每次保存的时候将代码按eslint格式进行修复

"eslint.format.enable": true,

//autoFix默认开启，只需输入字符串数组即可

"eslint.validate": [

"javascript",

"vue",

"html"

],

"editor.mouseWheelZoom": true,

// #让prettier使用eslint的代码格式进行校验

"prettier.eslintIntegration": true,

// #去掉代码结尾的分号

"prettier.semi": false,

// #使用带引号替代双引号

"prettier.singleQuote": true,

// #让函数(名)和后面的括号之间加个空格

"javascript.format.insertSpaceBeforeFunctionParenthesis": true,

// #这个按用户自身习惯选择

"vetur.format.defaultFormatter.html": "js-beautify-html",

// #让vue中的js按编辑器自带的ts格式进行格式化

"vetur.format.defaultFormatter.js": "vscode-typescript",

"vetur.format.defaultFormatterOptions": {

"js-beautify-html": {

"wrap_attributes": "force-aligned"

// #vue组件中html代码格式化样式

}

},

// 格式化stylus, 需安装Manta's Stylus Supremacy插件

"stylusSupremacy.insertColons": false, // 是否插入冒号

"stylusSupremacy.insertSemicolons": false, // 是否插入分好

"stylusSupremacy.insertBraces": false, // 是否插入大括号

"stylusSupremacy.insertNewLineAroundImports": false, // import之后是否换行

"stylusSupremacy.insertNewLineAroundBlocks": false,

"editor.codeActionsOnSave": {

"source.fixAll.eslint": "explicit"

}, // 两个选择器中是否换行

"editor.lineNumbers": "on", //开启行数提示

"editor.quickSuggestions": { //开启自动显示建议

"other": true,

"comments": true,

"strings": true

},

"window.zoomLevel": 0, // 调整窗口的缩放级别

"editor.formatOnType": true,

"comma-spacing": 0, //逗号前后的空格

"python.autoComplete.extraPaths": [

"/home/b/projects/ws_livox/devel/lib/python3/dist-packages",

"/home/b/projects/Fast-Drone-250/devel/lib/python3/dist-packages",

"/opt/ros/noetic/lib/python3/dist-packages"

],

"python.analysis.extraPaths": [

"/home/b/projects/ws_livox/devel/lib/python3/dist-packages",

"/home/b/projects/Fast-Drone-250/devel/lib/python3/dist-packages",

"/opt/ros/noetic/lib/python3/dist-packages"

],

"cmake.sourceDirectory": "/home/b/projects/autoland_ws/Firmware/ROMFS/px4fmu_common/mixers-sitl",

"files.associations": {

"iostream": "cpp",

"ostream": "cpp"

}

}