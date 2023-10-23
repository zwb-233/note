# java文件IO操作

* 使用Stream完成java文件IO基本操作
  * File类:文件绑定
  * FileInputStream/FileOutputStream类:单字符输出
  * BufferedInputStream/BufferedOutputStream类:缓冲区字符输出
  * BufferedReader/PrintReader类:字符串输出
  * ObjectOutputStream/ObjectInputStream类:对象序列化

```java
//文件读取操作
public class IoTest {

    /**
     * 文件的操作
     */
    public static void useFile(){
        File file = new File("src/main/resources/ioTest/1.txt");
        System.out.println(file.exists());
        System.out.println(file.getAbsolutePath());
        System.out.println(file.length());
        System.out.println(file.lastModified());

        //获取子文件名称
        String[] fileNames = file.list();
        //获取子文件对象
        File[] files = file.listFiles();
        for (File f:files) {
            System.out.println(f);
        }
    }

    /**
     * 使用普通IO流完成文件复制操作
     */
    public static void useIoStream() {
        File file1 = new File("src/main/resources/ioTest/1.txt");
        File file2 = new File("src/main/resources/ioTest/2.txt");
        if(file2.exists()){
            file2.delete();
        }

        FileInputStream in = null;
        FileOutputStream out = null;
        int now = -1;

        try {
            in = new FileInputStream(file1);
            out = new FileOutputStream(file2);
            while ((now = in.read()) != -1) {
                out.write(now);
            }
        } catch (IOException fileNotFoundException) {
            fileNotFoundException.printStackTrace();
        } finally {
            if (in != null) {
                try {
                    in.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if (out != null) {
                try {
                    out.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 使用bufferStream完成文件复制操作
     */
    public static void useIoBuffer() {
        File file1 = new File("src/main/resources/ioTest/1.txt");
        File file2 = new File("src/main/resources/ioTest/3.txt");
        if(file2.exists()){
            file2.delete();
        }

        FileInputStream in = null;
        FileOutputStream out = null;
        BufferedInputStream reader = null;
        BufferedOutputStream writer = null;

        try {
            in = new FileInputStream(file1);
            out = new FileOutputStream(file2);
            reader = new BufferedInputStream(in);
            writer = new BufferedOutputStream(out);
            int now = -1;
            byte[] cache = new byte[1024];
            while ((now = reader.read(cache))!= -1) {
                writer.write(cache,0,now);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (writer!= null) {
                try {
                    writer.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if (reader!= null) {
                try {
                    reader.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if (in!= null) {
                try {
                    in.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if(out!=null) {
                try {
                    out.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 使用Reader字符串流
     */
    public void useReader(){
        File file1 = new File("src/main/resources/ioTest/1.txt");
        File file2 = new File("src/main/resources/ioTest/4.txt");
        if(file2.exists()){
            file2.delete();
        }
        BufferedReader reader = null;
        PrintWriter writer = null;
        try {
            reader = new BufferedReader(new FileReader(file1));
            writer = new PrintWriter(file2);
            String line = null;
            while ((line = reader.readLine())!= null) {
                writer.println(line);
            }
            writer.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (writer!= null) {
                try {
                    writer.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if (reader!= null) {
                try {
                    reader.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 使用序列化保存和取出对象
     */
    public void useSerializableObj() {
        File file = new File("src/main/resources/ioTest/test.dat");
        if (file.exists()) {
            file.delete();
        }

        try {
            file.createNewFile();
        } catch (IOException e) {
            e.printStackTrace();
        }
        ObjectOutputStream out = null;
        ObjectInputStream in = null;
        FileOutputStream fOut = null;
        FileInputStream fIn = null;

        try {
            //序列化
            fOut = new FileOutputStream(file);
            out = new ObjectOutputStream(fOut);
            out.writeObject(new User(1, "hello", 18));
            out.flush();

            //反序列化
            fIn = new FileInputStream(file);
            in = new ObjectInputStream(fIn);
            Object user = in.readObject();
            System.out.println(user.toString());
        }catch (IOException | ClassNotFoundException e){
            e.printStackTrace();
        } finally{
            if(out!=null){
                try {
                    out.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if(in!=null) {
                try {
                    in.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if (fOut!=null) {
                try {
                    fOut.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if (fIn!=null) {
                try {
                    fIn.close();
                }catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

//序列化的类必须实现Serializable接口
class User implements Serializable {
    private static final long serialVersionUID = 1L;
    private int id;
    private String name;
    private int age;

    public User(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```
