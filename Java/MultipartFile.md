# MultipartFile



[toc]



方法

```java
package org.springframework.web.multipart;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import org.springframework.core.io.InputStreamSource;
import org.springframework.core.io.Resource;
import org.springframework.lang.Nullable;
import org.springframework.util.FileCopyUtils;

public interface MultipartFile extends InputStreamSource {
    
    // 前端传递名称
    String getName();

    // 源文件名
    @Nullable
    String getOriginalFilename();

    @Nullable
    String getContentType();

    boolean isEmpty();

    long getSize();

    byte[] getBytes() throws IOException;

    InputStream getInputStream() throws IOException;

    default Resource getResource() {
        return new MultipartFileResource(this);
    }

    void transferTo(File var1) throws IOException, IllegalStateException;

    default void transferTo(Path dest) throws IOException, IllegalStateException {
        FileCopyUtils.copy(this.getInputStream(), Files.newOutputStream(dest));
    }
}
```

