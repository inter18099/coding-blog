---
title: spring4 and hibernate5 configuration using javaconfig of nonweb application
category: Java
---

## Spring4 and Hibernate5-configuration using javaconfig of non web application

### Why I wrote this.

There are a lot of articles about configuring spring and hibernate using XML of web application. But few for it using javaconfig of non web application.

So I spend half a day test code and wrote this article.

BTW: Actually because I wanted to solve this problem : https://stackoverflow.com/questions/53157519/hibernate-can-insert-but-does-not-delete-entry , I started to try to do this.

*And I use HibernateTemplate to query database, which is not recommmanded.*

### Code for Main.java

```
@Controller
public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
        ctx.register(HibernateConf.class);
        ctx.refresh();
        TestEntity testEntity = new TestEntity();
        testEntity.setId(30);
        testEntity.setDescription("abc");
        ctx.getBean(TestEntityDAO.class).createEntity(testEntity);
        ctx.getBean(TestEntityDAO.class).deleteUseTemplate();
        //d.createEntity(1,"abc");
    }
}
```

I use AnnotationConfigApplicationContext, because this is a non web application. And I find that no annotation is provided for javaconfig style of "<annotation-driven>", so I use getBean instead of dependency inject.

### Code for HibernateConf.java

```
@Configuration
@EnableTransactionManagement
@ComponentScan(basePackages = {"com.sf.test"})
public class HibernateConf {

    @Autowired
    private EntityManagerFactory emFactory;
    @Bean
    public EntityManager getEntityManager() {
        return emFactory.createEntityManager();
    }

    @Bean
    public LocalSessionFactoryBean sessionFactory() {
        final LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
        sessionFactory.setDataSource(dataSource());
        sessionFactory.setPackagesToScan(new String[] { "com.sf.test" });
        sessionFactory.setHibernateProperties(hibernateProperties());

        return sessionFactory;
    }

    @Bean
    public DataSource dataSource() {
        final BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/jiujiu_test");
        dataSource.setUsername("root");
        dataSource.setPassword("wenwendaoabcdatabasepassword");

        return dataSource;
    }

    @Bean
    public PlatformTransactionManager hibernateTransactionManager() {
        final HibernateTransactionManager transactionManager = new HibernateTransactionManager();
        transactionManager.setSessionFactory(sessionFactory().getObject());
        return transactionManager;
    }

    private final Properties hibernateProperties() {
        final Properties hibernateProperties = new Properties();
        hibernateProperties.setProperty("hibernate.hbm2ddl.auto", "update");
        hibernateProperties.setProperty("hibernate.dialect", "org.hibernate.dialect.MySQLDialect");

        return hibernateProperties;
    }
}
```

You do not want to omit @EnableTransactionManagement which is necessary, because without it you may get a transaction exception.

### Code for TestEntityDAO.java

```
@Repository
public class TestEntityDAO {

    @Autowired
    private SessionFactory sessionFactory;

    @Transactional
    public void createEntity(TestEntity entity) {
        getCurrentSession().save(entity);
    }

    @Transactional
    protected Session getCurrentSession() {
        return sessionFactory.getCurrentSession();
    }

    @Transactional
    public void deleteUseTemplate() {
        HibernateTemplate template = new HibernateTemplate(sessionFactory);
        TestEntity e = template.load(TestEntity.class, 6);
        try {
            template.delete(e);
        } catch (Exception ex) {
            System.out.println("hahahahahaha");
            ex.printStackTrace();
        }
        //template.flush();
    }
}

```

What template.load() method do is use the id parameter to get content in test_entity table.

@Transaction of each method is necessary, without it some expection may happen.

### Code for TestEntity.java

```
@Entity
@Table(name = "test_entity")
public class TestEntity {
    @Id
    @Column(name = "id")
    private int id;
    @Column(name = "description")
    private String description;


    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }
}
```


### table structure of test_entity

```
CREATE TABLE `test_entity` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `description` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

```

You can check entire code in GitHub repository: https://github.com/inter18099/Spring4-and-Hibernate5-configuration-using-javaconfig-of-non-web-application.