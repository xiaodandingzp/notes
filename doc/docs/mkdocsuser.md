#参考链接
[Mkdocs 配置和使用](https://zhuanlan.zhihu.com/p/383582472)

#常用命令
##本地测试
* mkdir docs 创建docs文件夹
* cd docs
* mkdocs new .
* mkdocs build
* mkdocs serve
##发布
1. 代码推到远端
2. mkdocs gh-deploy --clean

#文档地址
https://{username}.github.io/{projectname}  
https://xiaodandingzp.github.io/MyToDoApplication/

#打开首页是404页面
index.md是打开的首页，要有该文件，不然找不到首页。