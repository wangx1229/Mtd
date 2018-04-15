# webpack4 零经验入门踩坑指南
1. 首先使用命令行cnpm init -y 快速生成我们项目的配置文件（package.json） 
2. 配置文件是严格的json文件 所以只能使用双引号
3. 在scrip值时 可以在末ts属性里面设置尾添加 --mode production 或者 --mode development 来设置webpack.js的运行模式
   其中 --mode development 生成开发环境的js 代码不压缩
   而   --mode production  生成生产环境的js 代码会压缩
   不设位置mode 会默认压缩

```javascript
   "scripts": {
     "xxx": "xxx --mode development/production/none"
   },
```

关于配置babel 按着官方文档做法会报出文件中不存在的错误 查了一下babel的说明 正确写法应该这么来
```javascript
rules: [
	{
    test: /\.js$/,
	 	exclude: /node_modules/,
	 	use: {
	  	loader: 'babel-loader',
	 	 	options: {
				presets: ['env']，
	     	plugins: ['transform-runtime']
	  	} 
  	}
	}
]
```
