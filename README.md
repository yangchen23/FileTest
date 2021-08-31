# FileTest

```java
package com.file.demo;

import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.*;
import java.util.*;

/**
 * The <code>com.file.demo.FileUtil</code> class have to be described
 *
 * <p>
 * The <code>FileUtil</code> class have to be detailed For example:
 * <p>
 *
 * @author ygcn_0823
 * @date 2021/8/31 22:11
 * @see
 * @since 1.0
 */
public class FileUtil {

    public FileUtil(){

    }

    /**
     * 读取txt文件，并统计指定字符串出现的次数
     */
    public String readFile(String path, String wen) {
        StringBuilder sb = new StringBuilder();
        try {
            int count = 0;
            int start = 0;
            BufferedReader br = new BufferedReader(new FileReader(path));
            String line = "";
            while ((line = br.readLine()) != null) {
                sb.append(line).append("\n");
                start = line.indexOf(wen);
                if (start != -1) {
                    count++;
                    start = line.indexOf(wen, start + 1);
                    while (start != -1) {
                        count++;
                        start = line.indexOf(wen, start + 1);
                    }
                }
            }
            sb.append(count);
            br.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return sb.toString();
    }


    /**
     * 拷贝zip文件
     */
    public void copyFile(String srcPath, String targetPath) {
        try {
            FileInputStream input = new FileInputStream(srcPath);
            FileOutputStream output = new FileOutputStream(targetPath);
            byte[] buf = new byte[1024];
            int byteRead;
            while ((byteRead = input.read(buf)) > 0) {
                output.write(buf, 0, byteRead);
            }
            input.close();
            output.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 解析properties文件
     */
    public void parsProFile(String path) {
        Map<String, String> map = new HashMap<>();
        Properties properties = new Properties();
        try {
            InputStream in = this.getClass().getResourceAsStream(path);
            properties.load(in);
            //读取文件中的键集
            Set<Object> keySet = properties.keySet();
            Iterator<Object> iterator = keySet.iterator();;
            while (iterator.hasNext()){
                String key = (String)iterator.next();
                String value = properties.getProperty(key);
                map.put(key, value);
            }
        }catch (IOException e){
            e.printStackTrace();
        }
    }

    /**
     * 解析xml文件
     * DOM4j
     */
    public void parsXmlFile(String path) {
        try {
            //创建Reader对象
            SAXReader reader = new SAXReader();
            //加载xml文件
            Document document = reader.read(new File(path));
            //获取根节点
            Element rootElement = document.getRootElement();
            Iterator iterator = rootElement.elementIterator();
            while (iterator.hasNext()) {
                Element stu = (Element) iterator.next();
                List<Attribute> attributes = stu.attributes();
                System.out.println("=====获取属性值=====");
                for (Attribute attribute : attributes) {
                    System.out.println(attribute.getValue());
                }
                System.out.println("=====遍历子节点");
                Iterator iterator1 = stu.elementIterator();
                while (iterator1.hasNext()) {
                    Element stuChild = (Element) iterator1.next();
                    System.out.println("节点名：" + stuChild.getName() + "---节点值：" + stuChild.getStringValue());
                }
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    }


}

```
