---
title: "原生js实现瀑布流布局"
categories: [ "默认" ]
tags: [ "JavaScript" ]
draft: false
slug: "10"
date: "2021-06-16 09:14:00"
---

瀑布流的特点就是容器等宽不等高，计算其高度，选择最矮的一个容器的下面将插入第二行的第一个容器，以此类推，因此容器需要设置为绝对定位

首先需要确定一行有多少列，列数 = 页面的宽度 / 容器的宽度

获取页面的宽度：window.innerWidth或者document.documentElement.clientWidth或者document.body.clientWidth

获取页面的高度：window.innerHeight或者document.documentElement.clientHeight或者document.body.clientHeight

页面宽度：.width，容器宽度：.offsetWidth

得到了列数后，还需要得到全部容器的高度，因此还需要用到数组，用来存储容器的高度

遍历全部容器，还需要判断是否到了第二行，i<列数

而第一行的全部容器，设置头部和左边定位，左边定位设置为容器的宽度*i，保证不会被覆盖或者溢出，同时将arr[i].offsetWidth传入新的数组中

然后找到上一行最矮的容器，只需要将全部得到的容器的宽度判断一下就可以，首先假释一下第一个容器就是最矮的，当遍历的容器小于该容器时，那么将该容器设置为当前遍历的容器，而当前遍历的i就是最矮的容器索引值

然后就设置下一行的容器的位置就可以了，头部：最矮的容器的高度，左边：最矮的容器距离页面最左边的宽度


因为已经设置了下一行的容器，因此还需要重新获取一下当前容器的高度，当前容器高度 = 当前容器高度+间隙 +拼接过来的容器的高度 


为了体验更好，可以将上面操作封装到一个函数中，当网页加载完毕加执行（window.onload），当页面宽度高度发送变化时也执行(window.onresize)





例如：

    var data = document.getElementsByClassName('imgs');
    function datamain(){
        var datawidtha = window.innerWidth;
        var datawidthb = Math.floor(data[0].offsetWidth);
        var dataab = Math.floor(datawidtha/datawidthb);
        var ints = Math.floor((datawidtha - dataab*datawidthb)/(dataab+1))
        var arr = [];
        for(var i=0;i<data.length;i++){
            if(i<dataab){
                data[i].style.top = 0;
                data[i].style.left = (datawidthb+ints) * i + "px";
                arr.push(data[i].offsetHeight);
            }else{
                var datamin = arr[0];
                var index = 0;
                for(var a = 0; a<arr.length; a++){
                    if(datamin > arr[a]){
                        datamin = arr[a];
                        index = a;
                    }
                }
                data[i].style.top = arr[index]+ ints +"px";
                data[i].style.left = data[index].offsetLeft + "px";
                arr[index] = arr[index] + data[i].offsetHeight + ints;
            }
        }
    }
    window.onload = function () {
        datamain();
    }
    window.onresize = function () {
        datamain();
    }