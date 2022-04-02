# ClickHouse

参考资料：

1. [ClickHouse JDBC github](https://github.com/ClickHouse/clickhouse-jdbc#jdbc-driver)
2. 

## JDBC连接CH


> In addition, starting from 0.3.2, JDBC driver only works with ClickHouse 20.7 or above, so please consider to either downgrade the driver to 0.3.1-patch or upgrade server to one of active releases.

1. JDBC 0.32以上的版本只支持CH20.7版本以上，JDBC 0.32版本一下的也支持CH20.7。
2. 另外，32以后版本只有`ru.yandex.clickhouse.ClickHouseDriver`版本，没有`ru.yandex.clickhouse`版本了。可以在maven库中搜索看到答案。

代码
`POM.xml`添加
```xml
        <!-- https://mvnrepository.com/artifact/com.clickhouse/clickhouse-jdbc -->
        <dependency>
            <groupId>com.clickhouse</groupId>
            <artifactId>clickhouse-jdbc</artifactId>
            <version>0.3.2</version>
        </dependency>
```

```Java

public class CKDemo {
    // JDBC 驱动名及数据库 URL
    static final String url = "jdbc:ch://xxxx:xx/xxx";
    // 数据库的用户名与密码，需要根据自己的设置
    static final String USER = "xxx";
    static final String PASS = "xxx";
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        Properties systemProperties = System.getProperties();
        // 代理IP端口，如果需要才要设置
        //systemProperties.setProperty("socksProxyHost","xxx");
        //systemProperties.setProperty("socksProxyPort","xx");
        // 打开链接
        System.out.println("连接数据库...");
        try{
            conn = DriverManager.getConnection(url, USER, PASS);
            stmt = conn.createStatement();
            // 执行查询
            System.out.println(" 实例化Statement对象...");
            ResultSet rs = stmt.executeQuery("select k1,k2 from xxxx.xxxx") ;
            // 展开结果集数据库
            while(rs.next()){
                // 通过字段检索
                int venderId  = rs.getInt("k1");
                int title = rs.getInt("k2");
                // 输出数据
                System.out.print("K1: " + venderId);
                System.out.print("K2: " + title);
                System.out.print("\n");
            }
            // 完成后关闭
            rs.close();
            stmt.close();
            conn.close();
        }catch(SQLException se){
            // 处理 JDBC 错误
            se.printStackTrace();
        }catch(Exception e){
            // 处理 Class.forName 错误
            e.printStackTrace();
        }finally{
            // 关闭资源
            try{
                if(stmt!=null) stmt.close();
            }catch(SQLException se2){
            }// 什么都不做
            try{
                if(conn!=null) conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
        System.out.println("Goodbye!");
    }
}

```

## MySQL 连接 CH

堡垒机上可以执行`mysql -h xxxx -P xxxx -u xxxx -pxxxx`


## CH 建表

必须同时建立本地表和分布式表。
例如

```sql

CREATE TABLE   ind_trend.table_name_local ON CLUSTER xxxx
(
    k1 UInt32,
    k2 UInt32
) ENGINE = ReplicatedMergeTree('/clickhouse/xxx/xxx/xxx/table_name_local/{shard}', '{replica}')
ORDER BY ( k1, k2)
SETTINGS storage_policy ='xxxx';

```

再建立分布式表

```sql

CREATE TABLE  IF NOT EXISTS xxxx.table_name_local_dis ON CLUSTER xxxx
AS ind_trend.table_name_local ENGINE = Distributed(xxxx, xxxx , table_test_local_2 , rand());

```

