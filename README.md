# 这是一级标题

### InputStream重复读取

**标准的 InputStream 本身不支持重复读取**，但可以通过一些方法实现重复读取的效果。InputStream 是单向数据流，读取位置会随着读取操作不断前进，无法回退到之前的位置。

将流数据读取到字节数组中，然后多次创建ByteArrayInputStream。

```java
public class RepeatableInputStream {
    private final byte[] data;
    private int position = 0;
    
    public RepeatableInputStream(InputStream inputStream) throws IOException {
        this.data = inputStream.readAllBytes();
        inputStream.close();
    }
    
    public InputStream getNewStream() {
        return new ByteArrayInputStream(data);
    }
    
    public void reset() {
        this.position = 0;
    }
    
    public int read() {
        if (position >= data.length) {
            return -1;
        }
        return data[position++] & 0xFF;
    }
    
    public byte[] getData() {
        return data.clone();
    }
}
```

