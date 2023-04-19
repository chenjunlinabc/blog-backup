---
title: "IndexedDB浏览器数据库学习笔记"
categories: [ "默认" ]
tags: [ "JavaScript" ]
draft: false
slug: "5"
date: "2021-06-15 19:00:00"
---

IndexedDB是浏览器提供的本地数据库，允许存储数据，采用键值对存储，允许异步请求，允许索引请求，IndexedDB理论上存储空间没有限制

打开（新建）indexedDB：

indexedDB.open()，该方法有俩个值，第一个是数据库名（没有该数据库那么就新建），第二个值为数据库的版本，一般来说默认为1（必须是大于0的自然数），例如：


    var datadb = window.indexedDB.open("data",1);
    datadb.onerror = function(){
        console.log("数据库打开失败")
    }
    datadb.onsuccess = function(){
        console.log("数据库打开成功");
    }
    datadb.onupgradeneeded = function(event){ 
        console.log("数据库版本升级");
    }


onerror事件表示打开数据库失败

onsuccess事件表示打开数据库成功

onupgradeneeded事件表示数据库升级（当数据库版本低于指定的数据库版本）（另外，新建数据库，操作新的数据库也可以在该事件中处理）



新建数据对象（数据表）

    datadb.onupgradeneeded = function(event) {
        console.log("数据库升级");
        var objdb = event.target.result.createObjectStore("abc",{keyPath: "id"});
    }

表名为abc，主键为id，主键默认为索引


判断是否存在该表

    if(!event.target.result.objectStoreNames.contains('abc')){
        var objdb = event.target.result.createObjectStore("abc",{keyPath: "id"});
    }


递增属性(可以用于作为主键)

    var maindb = event.target.result.createObjectStore("abc",{autoIncrement: true});

唯一属性

    objdb.createIndex("id","id",{unique: false})

createIndex()，三个参数分别是索引名称，索引的属性，配置对象，用来新建索引


添加数据


    var datadb = window.indexedDB.open("data",1);
    var db;
    var objdb;
    datadb.onupgradeneeded = function(event) {
        console.log("数据库升级");
        db = event.target.result;
        console.log(db)
        
        if(!event.target.result.objectStoreNames.contains('abc')){
            objdb = event.target.result.createObjectStore("abc",{keyPath: "id"});
        }
    }
    datadb.onsuccess = function (event) {
        console.log("数据添加成功");
        var tp = event.target.result;
        var transaction = tp.transaction(['abc'], 'readwrite');
        var objadd = transaction.objectStore('abc');
        objadd.add({id: 1,name: 'a', email: "a@xiaochenabc123.test.com"});
    }   

 

只要通过一个叫事务（transaction）来操作数据库，有两个参数，分别是要操作的数据对象，事务模式

事务模式有三种：

readOnly：只读
readwrite：读写
versionchange：数据库版本变化

不指定则默认为readOnly（只读）


添加多条数据


    var arrdata = [
        {id: 1,name: 'a', email: "a@xiaochenabc123.test.com"},
        {id: 2,name: 'b', email: "b@xiaochenabc123.test.com"},
        {id: 3,name: 'c', email: "c@xiaochenabc123.test.com"}
    ];
    datadb.onsuccess = function (event) {
        console.log("数据库添加成功");
        var tp = event.target.result;
        var transaction = tp.transaction(['abc'], 'readwrite');
        var objadd = transaction.objectStore('abc');
        for(var i in arrdata){
            objadd.add(arrdata[i]);
        }
    }   


读取数据


    var datadb = window.indexedDB.open("data",4);   
   
    datadb.onsuccess = function (event) {
        console.log("数据库读取成功");
        var ab = event.target.result;
        var transactiona = ab.transaction(['abc']);
        var objectStorea = transactiona.objectStore('abc');
        var datas = objectStorea.get(1); // objectStorea.get()方法的参数为主键的值
        datas.onsuccess = function( event) {
            if (datas.result) {
            console.log('ID: ' + datas.result.id);
            console.log('Name: ' + datas.result.name);
            console.log('Email: ' + datas.result.email);
           }else {
            console.log('未读取到数据记录');
           }
        };  
        datas.onerror = function(event) {
          console.log('数据库读取成功');
        };
    }



读取多条数据（遍历）


    var datadb = window.indexedDB.open("data",4);   
    datadb.onsuccess = function(){
        var db= event.target.result;
        displayData();
        function displayData() {
            var transaction = db.transaction(["abc"], "readonly");
            var objectStore = transaction.objectStore('abc');
            objectStore.openCursor().onsuccess = function(event) {
            var datas = event.target.result;
            if(datas) {
               console.log("id:"+datas.key);
               console.log("name:"+datas.value.name);
               console.log("email:"+datas.value.email)
               console.log(datas.value)
               datas.continue();
               
            } 
        }
    }

更新数据

和add()类似


    datadb.onsuccess = function (event) {
        console.log("数据更新成功");
        var tp = event.target.result;
        var transaction = tp.transaction(['abc'], 'readwrite');
        var objadd = transaction.objectStore('abc');
        objadd.put({id: 1,name: 'hallo', email: "abc@xiaochenabc123.test.com"})
    }   




删除数据

    datadb.onsuccess = function (event) {
        console.log("数据更新成功");
        var tp = event.target.result;
        var transaction = tp.transaction(['abc'], 'readwrite');
        var objadd = transaction.objectStore('abc');
        objadd.delete(1);
        console.log('数据删除成功');
    }   