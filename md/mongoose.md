# mongodb的学习 #
### 一、名词解释 ###
1. Schema: 一种以文件的形式存储数据库模型骨架，不具备数据库操作能力（数据库骨架）
2. Model: 由Schema发布生成的模型，具备抽象属性和行为的数据库操作对（数据库模型）
3. Entity： 由Model创建的实体，他的操作也会影响数据库(实体)
    Schema、Model、Entity的关系请牢记，Schema生成Model，Model创造Entity，Model和Entity都可对数据库操作造成影响，但Model比Entity更具操作性
### 二、 Mongoose的使用 ###
1. 必须安装Mogodb和node
2. 创建数据库

    const mongoose = require('mongoose')
	const db = mongoose.createConnecttion('localhost','mydb')
3. 打开数据库，并监测是否连接异常
    db.on('error',console.error.bind(console,'连接错误'))
	db.once("open",()=console.log("数据库连接成功"))
4. 定义Schma
    const Schema =  mongoose.Schema;
	const BlogSchema = new Schema({
	  title:  String,
	  author: String,
	  body:   String,
	  comments: [{ body: String, date: Date }],
	  date: { type: Date, default: Date.now },
	  hidden: Boolean,
	  meta: {
	    votes: Number,
	    favs:  Number
	  }
	});
5. 将该Schema发布Model
    var blogModel = db.model("blog",BlogSchema)
6. 用Model创建Entity
    var blogEntity = new blogModel({title: "Mongoose的学习"})
	console.log(blogEntity.title)
7. 为Schema创建方法
	BlogSchema.methods.sayAuther = function(){
		console.log('文章作者'+this.author);
	}
	var blogModel = db.model("blog",BlogSchema)
	var blogEntity = new blogModel({author: "小明"})
	blogEntity.sayAuther();
8. Entity 具有数据库操作
    blogEntity.save() // 保存数据
9. 执行查询,Model和Entity都可以做到
	blogModel.find({},(err,data){
		console.log(data)
	})
    
	