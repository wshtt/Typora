# epub (epub 文件并不是很规范，所以解析的时候要考虑多种情况)

[TOC]



### 一、 组成

    META-INF文件夹（meta-information）元信息
    OEBPS文件夹 包含images,xhtml,css,content.opf文件
    mimetype文件 内容为 application/epub+zip

  1. META-INF 文件夹
     

    用于存放电子书信息
    包含一个 container.xml 文件，该文件告诉电子书阅读器，文件的根文件路径和格式。
    此文件夹下还可以有其他可选文件：
          manifest.xml 文件列表
          metadata.xml 元数据
          sigatures.xml 数字签名
          encryption.xml 加密
          rights.xml 权限管理
  2. OEBPS 文件夹   （开放式电子图书出版结构）

    存放电子书真正内容：包括content.opf(内容),toc.ncx (目录)，正文，css样式，字体，封面，图片。
    
    opf，最重要的文件，标准的xml结构，根元素<package>

  2.1 opf文件

    第一部分 <metadata> 元数据
    包括两部分
    dc:metadata 元素，使用 Dublin Core， 包含 15 项核心元素：
    
        dc:title
        dc:creator 责任者
        dc:subject 主题关键词
        dc:description
        dc:publisher
        dc:contributor
        dc:date
        dc:type
        dc:format
        dc:identifier
        dc:source 来源
        dc:language
        dc:relation
        dc:coverage 覆盖范围
        dc:rights 权限描述
        
    meta 标签，扩展元素，如果有信息在上面标签中无法描述，则扩展到该 meta 中
    
    第二部分 <manifest> 文件列表
    每一行代表一个文件，由一个item构成
    <item id="ncx" href="toc.ncx" media-type="application/x-dtbncx+xml"/>
          id 为文件 id
          href 为文件相对路径
          media-type 为文件的媒体类型
    
    第三部分 <spine toc="ncx"> （脊柱，书脊）提供阅读顺序
    <spine toc="ncx">
        <itemref idref="cover" />
        <itemref idref="copyright" />
    </spine>
    idref为 第二部分的 id
    
    第四部分 <guide> (指南，向导) 列出了电子书的特定页面，比如封面，目录，序言等等，属性值指向文件地址。可选。
    
    第五部分，<tour> 导读，根据读者的不同水平，按照一定次序选择电子书部分页面组成导读，可选。

  2.2 ncx文件

    该文件用于电子书的目录，文件命名通常为 toc.ncx，ncx 文件也是一个 xml 文件。ncx 全称为 Navigation Center eXtended。
    根元素为<nxc>
    
    第一部分 <head>
    
    第二部分 <docTitle>
    
    第三部分 <navMap>
      内部是多个<navPoint>
        navPoint 节点中，playOrder 属性定义当前项在目录中的次序，text 子节点则定义了目录的名字
        content 子节点 src 属性定义了章节文件的具体位置，路径
    ***navPoint 节点可以嵌套，形成了整本书的层级结构。
## epub编辑器

https://github.com/Sigil-Ebook/Sigil/releases/tag/1.4.1