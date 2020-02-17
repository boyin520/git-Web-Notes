```
body,main::before{
  background: url("图片路径") 0 / cover fixed;      
}

main{ 
  position: relative;
  background: hska(0,0%,100%,.3);
  overflow: hidden;   
}

main::before{
  content: '',
  position: absolute;
  top:0;right:0;bottom:0;left:0;
  filter: blur(20px);
  margin:-30px;      
}
```

# [用CSS制作毛玻璃效果（高斯模糊效果）](https://www.cnblogs.com/zhangyepeng/p/8820732.html)