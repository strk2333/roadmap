# JavaEE





























JavaEE
Servlet
- Web.xml
  <servlet>
      <servlet-name>myservlet</servlet-name>
      <servlet-class>com.java1234.model.MyServlet</servlet-class>
  </servlet>
    <servlet-mapping>
      <servlet-name>myservlet</servlet-name>
      <url-pattern>/hello</url-pattern>
  </servlet-mapping>
- 测试：extends HttpServlet doGet doPost PrintWriter

Struts2

- Web.xml
     <filter>
        <filter-name>Struts2</filter-name>
        <filter-class>
            org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
        </filter-class>
     </filter>

    <filter-mapping>
        <filter-name>Struts2</filter-name>
        <url-pattern>/*</url-pattern>
　　</filter-mapping>
- Struts.xml
<struts>
    <package name="manager" extends="struts-default">
        <action name="student" class="com.java1234.action.StudentAction" method="add">
            <result name="success">/success.jsp</result>
            <!-- <result name="invalid.token">/student.jsp</result>
            <interceptor-ref name="token"></interceptor-ref>
            <interceptor-ref name="defaultStack"></interceptor-ref> -->
            <interceptor-ref name="tokenSession"></interceptor-ref>
            <interceptor-ref name="defaultStack"></interceptor-ref>
        </action>
    </package>
</struts>

- class extends ActionSupport

Hibernate
- hibernate.cfg.xml
<session-factory>

        <!--数据库连接设置 -->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://localhost:3306/hibernate</property>
        <property name="connection.username">root</property>
        <property name="connection.password">root</property>


        <!-- 方言 -->
        <property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>
    
        <!-- 控制台显示SQL -->
        <property name="show_sql">true</property>
    
        <!-- 自动更新表结构 -->
        <property name="hbm2ddl.auto">update</property>
    
         <!-- 启用二级缓存 -->
        <property name="cache.use_second_level_cache">true</property>
    
        <!-- 配置使用的二级缓存的产品 -->
        <property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property>
    
        <!-- 配置启用查询缓存 -->
        <property name="cache.use_query_cache">true</property>
    
          <mapping resource="com/java1234/model/Student.hbm.xml"/>
    
          <mapping resource="com/java1234/model/Class.hbm.xml"/>
    
    </session-factory>
- 测试 buildSessionFactory
        session = sessionFactory.openSession();
        session.beginTransaction();
        Student s1 = new Student();
        s1.setName("Kevin");
        session.save(s1);

        session.getTransaction().commit();
        session.close();
        sessionFactory.close();

- Student.hbm.xml
<hibernate-mapping package="com.java1234.model">

    <class name="Student" table="t_student">
        <id name="id" column="stuId">
            <generator class="native"></generator>
        </id>

        <property name="name" column="stuName"></property>
    
        <many-to-one name="c" column="classId" class="com.java1234.model.Class" cascade="save-update"></many-to-one>
    </class>

</hibernate-mapping>