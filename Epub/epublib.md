# epublib



[toc]

##### 简介

Epublib是一个用于管理epub文件的java库。它能够通过编程和命令行工具读写epub文件。

##### 下载

`http://www.siegmann.nl/epublib/download`

`http://github.com/psiegman/epublib/issues`

#### 基本使用

##### 读取 epub 文件

```java
EpubReader epubReader = new EpubReader();

// 获取Book 对象，Book 对象就是整个epub 文件对象
Book book = epubReader.readEpub(file.getInputStream());
```

##### 修改 epub 文件

```java
// 设置 title
book.getMetadata().setTitles(
    new ArrayList<String>() {
        {
            add("an awesome book");
        }
    });
```

##### 写出 epub 文件

```java
EpubWriter epubWriter = new EpubWriter();
// 写出 epub 文件
epubWriter.write(book, new FileOutputStream("mynewbook.epub"));
```

##### 写一本电子书案例

```java
package com.example.demo01.qianji.service.impl.ZipFile;

import nl.siegmann.epublib.domain.Author;
import nl.siegmann.epublib.domain.Book;
import nl.siegmann.epublib.domain.Resource;
import nl.siegmann.epublib.domain.TOCReference;
import nl.siegmann.epublib.epub.EpubWriter;

import java.io.FileOutputStream;


public class Simple1 {
    public static void main(String[] args) {
        try {
            // Create new Book
            Book book = new Book();

            // Set the title 书名
            book.getMetadata().addTitle("Epublib test book 1");

            // Add an Author 作者
            book.getMetadata().addAuthor(new Author("Joe", "Tester"));

            // Set cover image 封面图片 (InputStream href)
            // book.getMetadata().setCoverImage(new Resource(Simple1.class.getResourceAsStream("/book1/test_cover.png"), "cover.png"));

            // Add Chapter 1 第一章
            book.addSection("Introduction", new Resource(Simple1.class.getResourceAsStream("/book1/chapter1.html"), "chapter1.html"));

            // Add css file 添加css
            book.getResources().add(new Resource(Simple1.class.getResourceAsStream("/book1/book1.css"), "book1.css"));

            // Add Chapter 2 第二章
            TOCReference chapter2 = book.addSection("Second Chapter", new Resource(Simple1.class.getResourceAsStream("/book1/chapter2.html"), "chapter2.html"));

            // Add image used by Chapter 2 第二章图片
            book.getResources().add(new Resource(Simple1.class.getResourceAsStream("/book1/flowers_320x240.jpg"), "flowers.jpg"));

            // Add Chapter2, Section 1 第二章第一节
            book.addSection(chapter2, "Chapter 2, section 1", new Resource(Simple1.class.getResourceAsStream("/book1/chapter2_1.html"), "chapter2_1.html"));

            // Add Chapter 3 第三章
            book.addSection("Conclusion", new Resource(Simple1.class.getResourceAsStream("/book1/chapter3.html"), "chapter3.html"));

            // Create EpubWriter
            EpubWriter epubWriter = new EpubWriter();

            // Write the Book as Epub 写出
            epubWriter.write(book, new FileOutputStream("test1_book1.epub"));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### book 对象的方法

##### 获取书籍标题

```java
// 第一个为书籍的标题 
// 获取书籍标题 (content.opf 中的title；toc.ncx 中的 docTitle)
List<String> titles = book.getMetadata().getTitles();
System.out.println("book title:" + (titles.isEmpty() ? "book has no title" : titles.get(0)));

```

##### getContents

```java
// 所有章节内容  包括（书脊 Spine；目录 TableOfContents ；特殊页面，如目录，图片目录，封面 Guide）
List<Resource> contents = book.getContents();
```

##### getResources

```java
// 书籍所有的资源文件 （content.opf 中的 manifest）
Resources resources = book.getResources();
```

##### getSpine

```java
// 书脊 通常不包括封面 （content.opf 中的 spine; toc.ncx 中的 navMap)
Spine spine = book.getSpine();
List<SpineReference> spineReferences = spine.getSpineReferences();
```

##### getTableOfContents

```java
// 目录 目录可以有子目录，TableOfContents是一个大的对象
TableOfContents tableOfContents = book.getTableOfContents();
List<TOCReference> tocReferences = tableOfContents.getTocReferences();
```

##### getNcxResource

```java
// 获取 toc.ncx
Resource ncxResource = book.getNcxResource();
```

##### getOpfResource

```java
// 获取 content.opf
Resource opfResource = book.getOpfResource();
```

##### getTitle

```java
// title
String title = book.getTitle();
```

##### getCoverPage

```java
// 第一页 或者封面页
Resource coverPage = book.getCoverPage();
```

##### getCoverImage

```java
// 封面图片 （jar 里面根本没有设置封面图片的方法。。。）
Resource coverImage = book.getCoverImage();
```

##### getMetadata

```java
// metadata 书籍描述信息，写作时间、作者信息、等等
Metadata metadata = book.getMetadata();
```

##### getGuide

```java
// 特殊页面，如目录，图片目录，封面 (没什么用)
Guide guide = book.getGuide();
```



## 使用过的代码

```java
// 上传 epub

public EnterChapterBookDTO uploadEpub(MultipartFile file) {
    // 书籍id
    Long bookId = 0L;
    List<Map<String, Object>> epubList = new ArrayList<>();
    List<ChapterEntity> chapterEntityList = new ArrayList<>();
    List<Map<String, Object>> mapArrayList = new ArrayList<>();
    List<Map<String, Object>> imageList = new ArrayList<>();

    try {
        // 获取epub 图书流文件
        InputStream inputStreamEpub = file.getInputStream();
        if (inputStreamEpub == null) {
            throw new QJException("获取电子书流文件失败");
        }
        String filename = file.getOriginalFilename();
        int index = filename.lastIndexOf(".");
        String sub = filename.substring(0, index);
        String substring = changeToPinYin.getStringPinYin(sub);
        // 电子书储存目录
        String path = "BOOK/" + substring + "/";
        String epubPath = path + substring + "/" + file.getOriginalFilename().substring(index);

        //上传 epub图书
        Map<String, Object> epubMap = new HashMap<>();
        epubMap.put("path", epubPath);
        epubMap.put("inputStream", inputStreamEpub);
        epubList.add(epubMap);
        if (epubList != null && !epubList.isEmpty()) {
            // 保存书籍
            BookEntity bookEntity = new BookEntity();
            bookEntity.setBookName(sub);
            bookEntity.setBookLink(ossUrl + epubPath);
            bookEntity.setCreateDate(new Date());
            bookService.insert(bookEntity);
            bookId = bookEntity.getBookId();
        } else {
            throw new QJException("无法上传epub，流文件与path不足");
        }
        Long finalBookId = bookId;

        //解析 epub 图片  获取电子书 book 对象
        EpubReader epubReader = new EpubReader();
        Book book = epubReader.readEpub(file.getInputStream());
        if (book == null) {
            throw new QJException("获取电子书 book 失败");
        }

        //章节名称 与xml
        byte[] ncx = book.getNcxResource().getData();
        Document parse = Jsoup.parse(new String(ncx));
        Elements navPoint = parse.getElementsByTag("navPoint");
        navPoint.stream().forEach(e -> {
            //章节序号
            String sequence = e.attr("playorder");
            String title = e.getElementsByTag("navLabel").eachText().get(0);
            //章节路径
            String href = e.getElementsByAttribute("src").attr("src");
            Resource byHref = book.getResources().getByHref(href);
            try {
                //将字节数组转换为字符串
                String res = new String(byHref.getData());
                String resRepair = repairContent(res, ossUrl + path);
                String textPath = "/data/" + path;
                // String textPath = "F:/" + path;
                File fileA = new File(textPath);
                if (!fileA.exists()) {
                    fileA.mkdirs();
                }
                String filePath = textPath + href.substring(5);
                Map<String, Object> map = new HashMap<>();
                map.put("filePath", filePath);
                map.put("resRepair", resRepair);
                mapArrayList.add(map);
                saveChapter(filePath, finalBookId, title, Integer.parseInt(sequence), chapterEntityList);
            } catch (IOException ioException) {
                ioException.printStackTrace();
                throw new QJException("上传xhtml失败");
            }
        });
        // 封面 封底
        Resource cover = book.getResources().getById("cover.xhtml");
        if (cover != null) {
            String coverImageFile = changeCoverImage(new String(cover.getData()), path);
            Map<String, Object> map = new HashMap<>();
            String filePath = "/data/" + path + "cover.xhtml";
            map.put("filePath", filePath);
            map.put("resRepair", coverImageFile);
            mapArrayList.add(map);

            saveChapter(filePath, finalBookId, "封面", 0, chapterEntityList);
        }
        Resource backcover = book.getResources().getById("backcover.xhtml");
        if (backcover != null) {
            String backcoverImageFile = changeCoverImage(new String(backcover.getData()), path);
            Map<String, Object> map = new HashMap<>();
            String filePath = "/data/" + path + "backcover.xhtml";
            map.put("filePath", filePath);
            map.put("resRepair", backcoverImageFile);
            mapArrayList.add(map);

            saveChapter(filePath, finalBookId, "封底", 0, chapterEntityList);
        }
        // 插入目录数据
        chapterService.insertBatch(chapterEntityList);

        // 获取所有资源文件路径
        Set<String> hrefs = (Set<String>) book.getResources().getAllHrefs();
        hrefs.stream().forEach(e -> {
            // 如果是 图片，oss 上传
            if (e.startsWith("Images")) {
                Map<String, Object> imageMap = new HashMap<>();
                Resource byHref = book.getResources().getByHref(e);
                InputStream inputStreamImage = null;
                try {
                    inputStreamImage = byHref.getInputStream();
                } catch (IOException ioException) {
                    ioException.printStackTrace();
                }
                String imagesPath = path + e;
                imageMap.put("path", imagesPath);
                imageMap.put("inputStream", inputStreamImage);
                imageList.add(imageMap);
            }
        });
    } catch (IOException e) {
        e.printStackTrace();
    }

    //开启线程
    Runnable runnable = () -> {
        if (epubList != null && !epubList.isEmpty()) {
            uploadEpubs(epubList, "application/epub+zip");
        } else {
            throw new QJException("无法上传epub，流文件与path不足");
        }
        if (imageList != null && !imageList.isEmpty()) {
            uploadEpubs(imageList, "image/jpeg");
        } else {
            //                System.out.println("无图片");;
        }
        if (mapArrayList != null && !mapArrayList.isEmpty()) {
            uploadXhtml(mapArrayList);
        }
    };
    new Thread(runnable).start();
    //获取返回数据
    EnterChapterBookDTO enterChapterBookDTO = new EnterChapterBookDTO();
    enterChapterBookDTO.setBookId(bookId);
    BookEntity bookEntity = bookService.selectById(bookId + "");
    enterChapterBookDTO.setEpubLink(bookEntity.getBookLink());
    enterChapterBookDTO.setEpubLinkId(bookEntity.getBookLink().substring(44));
    List<ChapterVoiceDTO> arrayList = new ArrayList<>();

    List<ChapterEntity> list = chapterService.getByBookId(bookId + "");
    list.stream().forEach(e->{
        ChapterVoiceDTO chapterVoiceDTO = new ChapterVoiceDTO();
        chapterVoiceDTO.setBookId(e.getBookId());
        chapterVoiceDTO.setChapter(e.getId());
        chapterVoiceDTO.setChapterName(e.getTitle());
        chapterVoiceDTO.setSequence(e.getSequence());
        arrayList.add(chapterVoiceDTO);
    });
    enterChapterBookDTO.setChapterNumber(list.size());
    enterChapterBookDTO.setChapterVoiceList(arrayList);
    return enterChapterBookDTO;
}

/**
     * 上传epub 相关文件
     */
public void uploadEpubs(List<Map<String, Object>> list, String contentType) {
    List<UploadDTO> uploadDTOList = OssFactory.build().uploadEpub(list, contentType);
    uploadDTOList.stream().forEach(e -> {
        OssEntity ossEntity = new OssEntity();
        ossEntity.setUrl(e.getUrl());
        ossEntity.setCreateDate(new Date());
        ossEntity.setBucketName("hb-wxhl-image");
        ossEntity.setMd5(e.getMd5());
        baseDao.insert(ossEntity);
    });
}

/**
     * 将img标签中的src进行替换
     * @param content html string内容
     * @param replace 需要在src中加入的前缀，替换../Images
     * @return
     */
public static String repairContent(String content, String replace) {
    String patternStr = "<img\\s*([^>]*)\\s*src=\\\"(.*?)\\\"\\s*([^>]*)>";
    Pattern pattern = Pattern.compile(patternStr, Pattern.CASE_INSENSITIVE);
    Matcher matcher = pattern.matcher(content);
    String result = content;
    while (matcher.find()) {
        String src = matcher.group(2);
        String replaceSrc = "";
        if (src.length() > 0) {
            replaceSrc = src.substring(3);
        }
        replaceSrc = replace + replaceSrc;
        result = result.replaceAll(src, replaceSrc);
    }
    return result;
}

/**
     * 保存书籍目录
     * @param filePath    文件绝对路径
     * @param finalBookId 书籍id
     * @param title       章节名称
     * @param sequence    章节序号 封面封底为 0
     */
private void saveChapter(String filePath, Long finalBookId, String title, Integer sequence, List<ChapterEntity> chapterEntityList) {
    //保存书籍目录
    ChapterEntity chapterEntity = new ChapterEntity();
    chapterEntity.setBookId(finalBookId);
    chapterEntity.setLink("/ebook" + filePath.substring(10));
    chapterEntity.setCreateDate(new Date());
    if (title != null) {
        chapterEntity.setTitle(title);
    }
    if (sequence != null) {
        chapterEntity.setSequence(sequence);
    }
    chapterEntity.setCurrency(0);
    chapterEntity.setChaptercover("0");
    chapterEntity.setIsVip(0);
    chapterEntity.setOrder(9);
    chapterEntity.setTotalpage(0);
    chapterEntity.setUneadble(0);
    chapterEntity.setPartsize(0);
    //            chapterEntity.setTime((int) System.currentTimeMillis());
    chapterEntity.setTime(0);
    if (title.equals("封面")) {
        chapterEntity.setFileType(1);
    } else if (title.equals("封底")) {
        chapterEntity.setFileType(2);
    } else {
        chapterEntity.setFileType(0);
    }
    chapterEntityList.add(chapterEntity);
}

/**
     * @Description 上传xhtml 到服务器
     */
public void uploadXhtml(List<Map<String, Object>> mapArrayList) {
    mapArrayList.stream().forEach(e -> {
        String filePath = (String) e.get("filePath");
        String resRepair = (String) e.get("resRepair");
        try {
            File fileB = new File(filePath);
            if (!fileB.exists()) {
                fileB.createNewFile();
            }
            BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(new FileOutputStream(fileB));
            bufferedOutputStream.write(resRepair.getBytes());
            //关闭
            bufferedOutputStream.close();
        } catch (IOException ioException) {
            ioException.printStackTrace();
            throw new QJException("xhtml上传失败");
        }
    });
}

/**
     * 封面与封底 图片链接替换
     * @param coverData 封面 html 文件
     */
private String changeCoverImage(String coverData, String path) {
    Document parse = Jsoup.parse(coverData);
    Elements image = parse.getElementsByTag("image");
    if (!image.isEmpty()) {
        image.stream().forEach(e -> {
            String attr = e.attr("xlink:href");
            e.attr("xlink:href", ossUrl + path + attr.substring(3));
        });
    }
    return parse.html();
}
```

