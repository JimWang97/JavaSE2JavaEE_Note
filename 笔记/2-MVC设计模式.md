# 2. MVC设计模式
- Model 模型 表示应用程序核心（比如数据库记录列表）
    - 是应用程序中用于处理应用程序数据逻辑的部分
    - 通常模型对象负责在数据库中存取数据
    - 代表了存取数据的对象 POJO
- View 视图 显示数据 （数据库记录）
    - 是应用程序中处理数据显示的部分
    - 通常视图是依据模型数据创建的
    - 数据的可视化
- Controller 控制器 处理输入（写入数据库记录）
    - 是应用程序中处理用户交互的部分。
    - 通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据
    - 控制器作用于模型和视图上。它控制数据流向模型对象，并在数据变化时更新视图。它使视图与模型分离开。
    - servlet相当于是控制器，起到转发的功能
MVC分层有助于管理复杂的应用程序
分层思想：强内聚、弱耦合

View(JSP) -> UI
Controller(servlet) -> UI
Model(JavaBean) <- Service 业务层
Model(JavaBean) <- Dao 数据访问层

Dao层：主要是做数据持久层的工作，负责与数据库进行联络的一些任务都封装在此，DAO层的设计首先是设计DAO的接口，然后就可在模块中调用此接口来进行数据业务的处理，而不用关心此接口的具体实现类是哪个类，显得结构非常清晰，DAO层的数据源配置。
```
// 负责与数据库的互动
public interface UserDao{
    void add(User user);
}
```

service层：主要负责业务模块的逻辑应用设计,Service层的业务实现，具体要调用到已定义的DAO层的接口，封装Service层的业务逻辑有利于通用的业务逻辑的独立性和重复利用性，程序显得非常简洁。 
```
// 将需求给Dao，让Dao去与数据库操作
public class UserServiceImpl implements UserService{
    private UserDao userDao;
    public void add(User user){
        userDao.add(user);
    }
}
```

dao层和service层关系：service层经常要调用dao层的方法对数据进行增删改查的操作，现实开发中，对业务的操作会涉及到数据的操作，而对数据操作常常要用到数据库，所以service层会经常调用dao层的方法。

-----
