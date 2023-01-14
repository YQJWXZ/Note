# Spring整合JUnit

使用Spring整合Junit专用的类加载器

```java
import com.xiaoyi.springDemo.ContainerDemo.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;

@RunWith(SpringJUnit4ClassRunnner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class BookServiceTest {
    @Autowired
    private BookService bookService;

    @Test
    public void testSave(){
        bookService.save();
    }
}
```