```java

import org.junit.jupiter.api.*;
import redis.clients.jedis.Jedis;

/**
 * Project name(项目名称)：redis_jedis_zset_command
 * Package(包名): PACKAGE_NAME
 * Class(类名): Redis
 * Author(作者）: mao
 * Author QQ：1296193245
 * GitHub：https://github.com/maomao124/
 * Date(创建日期)： 2022/4/19
 * Time(创建时间)： 10:17
 * Version(版本): 1.0
 * Description(描述)： Zset 命令
 * 它是一个 set，这保证了内部 value 值的唯一性，其次它给每个 value 添加了一个 score（分值）属性，
 * 通过对分值的排序实现了有序化。比如用 zset 结构来存储学生的成绩，value 值代表学生的 ID，score 则是的考试成绩。
 * 我们可以对成绩按分数进行排序从而得到学生的的名次。
 * <p>
 * <p>
 * 命令	                说明
 * ZADD	                用于将一个或多个成员添加到有序集合中，或者更新已存在成员的 score 值
 * ZCARD        	    获取有序集合中成员的数量
 * ZCOUNT	            用于统计有序集合中指定 score 值范围内的元素个数
 * ZINCRBY	            用于增加有序集合中成员的分值
 * ZINTERSTORE	        求两个或者多个有序集合的交集，并将所得结果存储在新的 key 中
 * ZRANGE	            返回有序集合中指定索引区间内的成员数量
 * ZRANGEBYLEX	        返回有序集中指定字典区间内的成员数量
 * ZRANGEBYSCORE    	返回有序集合中指定分数区间内的成员
 * ZRANK	            返回有序集合中指定成员的排名
 * ZREM	                移除有序集合中的一个或多个成员
 * ZREMRANGEBYRANK	    移除有序集合中指定排名区间内的所有成员
 * ZREMRANGEBYSCORE	    移除有序集合中指定分数区间内的所有成员
 * ZREVRANGE	        返回有序集中指定区间内的成员，通过索引，分数从高到低
 * ZREVRANK	            返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序
 * ZSCORE	            返回有序集中，指定成员的分数值
 * ZUNIONSTORE	        求两个或多个有序集合的并集，并将返回结果存储在新的 key 中
 */


public class Redis
{
    private static Jedis jedis;

    /**
     * Sets up.
     */
    @BeforeEach
    void setUp()
    {
        System.out.println("---------------------");
    }

    /**
     * Tear down.
     */
    @AfterEach
    void tearDown()
    {
        System.out.println("---------------------");
    }

    /**
     * Before all.
     */
    @BeforeAll
    static void beforeAll()
    {
        jedis = new Jedis("127.0.0.1", 6379);
        jedis.auth("123456");
        System.out.println("启动");
    }

    /**
     * After all.
     */
    @AfterAll
    static void afterAll()
    {
        if (jedis != null)
        {
            jedis.close();
        }
        System.out.println("关闭");
    }

    /**
     * Zadd.
     */
    @Test
    void zadd()
    {
        //将具有指定分数的指定成员添加到存储在 key 的排序集中。
        // 如果 member 已经是排序集的成员，则更新分数，并将元素重新插入正确的位置以确保排序。
        // 如果 key 不存在，则创建一个以指定成员为唯一成员的新排序集。
        // 如果键存在但不包含已排序的设置值，则返回错误。分数值可以是双精度浮点数的字符串表示。
        //时间复杂度 O(log(N))，其中 N 是排序集中的元素数
        jedis.zadd("zset1", 89.3, "张三");
        long l = jedis.zadd("zset1", 81.2, "李四");
        System.out.println(l);
        jedis.zadd("zset1", 92.5, "王五");
    }

    /**
     * Zcard.
     */
    @Test
    void zcard()
    {
        //返回排序后的集合基数（元素数）。
        // 如果键不存在，则返回 0，例如空排序集。
        // 时间复杂度 O(1)
        System.out.println(jedis.zcard("zset1"));
    }

    /**
     * Zcount.
     */
    @Test
    void zcount()
    {
        System.out.println(jedis.zcount("zset1", 85, 90));
        System.out.println(jedis.zcount("zset1", 80, 90));
        System.out.println(jedis.zcount("zset1", 80, 95));
    }

    /**
     * Zincrby.
     */
    @Test
    void zincrby()
    {
        System.out.println(jedis.zincrby("zset1", 2, "张三"));
        System.out.println(jedis.zincrby("zset1", 2.5, "张三"));
    }

    /**
     * Zrange.
     */
    @Test
    void zrange()
    {
        System.out.println(jedis.zrange("zset1", 1, 2));
    }

    /**
     * Zrange by score.
     */
    @Test
    void zrangeByScore()
    {
        System.out.println(jedis.zrangeByScore("zset1", 80, 90));
    }

    /**
     * Zrank.
     */
    @Test
    void zrank()
    {
        System.out.println(jedis.zrank("zset1", "张三"));
        System.out.println(jedis.zrank("zset1", "李四"));
        System.out.println(jedis.zrank("zset1", "王五"));
    }

    /**
     * Zrem.
     */
    @Test
    void zrem()
    {
        System.out.println(jedis.zrem("zset1", "张三"));
    }

    /**
     * Zrevrange.
     */
    @Test
    void zrevrange()
    {
        System.out.println(jedis.zrevrange("zset1", 0, 1));
    }

    /**
     * Zrevrank.
     */
    @Test
    void zrevrank()
    {
        System.out.println(jedis.zrevrank("zset1","李四"));
        System.out.println(jedis.zrevrank("zset1","王五"));
    }

    /**
     * Zscore.
     */
    @Test
    void zscore()
    {
        System.out.println(jedis.zscore("zset1","王五"));
        System.out.println(jedis.zscore("zset1","李四"));
    }
}


```