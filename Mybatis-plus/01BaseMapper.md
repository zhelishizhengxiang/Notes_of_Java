# 一、BaseMapper的作用和使用

1. 作用：**`BaseMapper` 是 MP 提供的一个通用 Mapper 接口，封装了单表 CRUD（增删改查）的常用操作，让你无需手动编写基础 SQL 和 Mapper 方法，直接继承就能快速实现单表操作，大幅简化开发**
2. 使用: **只需要让Mapper接口继承BaseMapper即可直接使用对单表的增删改查功能**

该接口提供的所有方法如下  
```java
/*
 * Copyright (c) 2011-2022, baomidou (jobob@qq.com).
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
package com.baomidou.mybatisplus.core.mapper;

import com.baomidou.mybatisplus.core.conditions.Wrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.core.toolkit.CollectionUtils;
import com.baomidou.mybatisplus.core.toolkit.Constants;
import com.baomidou.mybatisplus.core.toolkit.ExceptionUtils;
import org.apache.ibatis.annotations.Param;

import java.io.Serializable;
import java.util.Collection;
import java.util.List;
import java.util.Map;


/**
 * Mapper 继承该接口后，无需编写 mapper.xml 文件，即可获得CRUD功能
 * <p>这个 Mapper 支持 id 泛型</p>
 *
 * @author hubin
 * @since 2016-01-23
 */
public interface BaseMapper<T> extends Mapper<T> {

    /**
     * 插入一条记录
     *
     * @param entity 实体对象
     */
    int insert(T entity);

    /**
     * 根据 ID 删除
     *
     * @param id 主键ID
     */
    int deleteById(Serializable id);

    /**
     * 根据实体(ID)删除
     *
     * @param entity 实体对象
     * @since 3.4.4
     */
    int deleteById(T entity);

    /**
     * 根据 columnMap中的条件，删除记录
     *
     * @param columnMap 表字段 map 对象
     */
    int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

    /**
     * 根据 entity 条件，删除记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
     */
    int delete(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 删除（根据ID或实体 批量删除）
     *
     * @param idList 主键ID列表或实体列表(不能为 null 以及 empty)
     */
    int deleteBatchIds(@Param(Constants.COLLECTION) Collection<?> idList);

    /**
     * 根据 ID 修改
     *
     * @param entity 实体对象
     */
    int updateById(@Param(Constants.ENTITY) T entity);

    /**
     * 根据 whereEntity 条件，更新记录
     *
     * @param entity        实体对象 (set 条件值,可以为 null)
     * @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
     */
    int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);

    /**
     * 根据 ID 查询
     *
     * @param id 主键ID
     */
    T selectById(Serializable id);

    /**
     * 查询（根据ID 批量查询）
     *
     * @param idList 主键ID列表(不能为 null 以及 empty)
     */
    List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);

    /**
     * 查询（根据 columnMap 条件）
     *
     * @param columnMap 表字段 map 对象
     */
    List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

    /**
     * 根据 entity 条件，查询一条记录
     * <p>查询一条记录，例如 qw.last("limit 1") 限制取一条记录, 注意：多条数据会报异常</p>
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    default T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper) {
        List<T> ts = this.selectList(queryWrapper);
        if (CollectionUtils.isNotEmpty(ts)) {
            if (ts.size() != 1) {
                throw ExceptionUtils.mpe("One record is expected, but the query result is multiple records");
            }
            return ts.get(0);
        }
        return null;
    }

    /**
     * 根据 Wrapper 条件，判断是否存在记录
     *
     * @param queryWrapper 实体对象封装操作类
     * @return
     */
    default boolean exists(Wrapper<T> queryWrapper) {
        Long count = this.selectCount(queryWrapper);
        return null != count && count > 0;
    }

    /**
     * 根据 Wrapper 条件，查询总记录数
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    Long selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 entity 条件，查询全部记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录
     * <p>注意： 只返回第一个字段的值</p>
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 entity 条件，查询全部记录（并分页）
     *
     * @param page         分页查询条件（可以为 RowBounds.DEFAULT）
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    <P extends IPage<T>> P selectPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录（并分页）
     *
     * @param page         分页查询条件
     * @param queryWrapper 实体对象封装操作类
     */
    <P extends IPage<Map<String, Object>>> P selectMapsPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
}
```
要点：  
* **Mybatis-plus所操作的表以及当前表中的字段是由BaseMapper泛型中指明要操作的实体类和实体类的属性决定**
* **支持链式查询 / 条件构造**：结合 MP 的 `Wrapper` 条件构造器，能灵活实现复杂的单表条件查询（如多条件筛选、排序、分页）


# 二、测试BaseMapper的增删改查

###### 插入功能
```java
/** 测试BaseMapper的新增功能 **/
@Test
public void testInsert() {
    // 实现新增用户信息
    // INSERT INTO user ( id, name, age, email ) VALUES ( ?, ?, ?, ? )
    User user = new User();
    // user.setId鲤L);
    user.setName("张三";);
    user.setAge("12");
    user.setEmail("zhangsan@atguigu.com");
    int result = userMapper.insert(user);
    System.out.println("result = " + result);
    System.out.println("id = " + user.getId());
}
```
运行结果：    
![[file-20260107151544962.png]]
* id不是正常情况下的原因：**MyBatis-Plus在实现插入数据时，会默认基于雪花算法（后面会讲）的策略生成id


###### 删除功能

条件构造器Wrapper后面会专门讲，所以此处就测试另外三个方法
 
1. 通过id删除用户信息  
![[file-20260107152110518.png]]
![[file-20260107152212409.png]]

2. 根据map中的条件删除用户  
![[file-20260107152420562.png]]
![[file-20260107152503231.png]]
注：结果为0是因为表中没有此数据

3. 通过id进行批量删除    
![[file-20260107153000485.png]]
![[file-20260107152839234.png]]


###### 更新功能
条件构造器有详细的讲解，所以此处不测试update()，只测试updateById(T entity)

![[file-20260107153352224.png]]
![[file-20260107153412399.png]]

###### 查询功能

同样的，根据条件构造器Warpper构建条件查询的api我们不做测试，后面会专门讲解Wrapper

1. 根据id查询用户信息  
 ![[file-20260107154131735.png]]
![[file-20260107154120746.png]]

2. 批量查询    
![[file-20260107154224893.png]]
![[file-20260107154316241.png]]

3. 根据mao中的条件查询用户    
![[file-20260107154410390.png]]
![[file-20260107154502307.png]]


4. 查询所有数据   
![[file-20260107154612959.png]]
![[file-20260107154702462.png]]
* 当没有条件时，条件构造器为null即可

###### 自定义功能

MP的BaseMapper只是封装了单表查询相关内容，对于多表查询仍然需要进行自定义操作。

![[file-20260107160638221.png]]
* **但是在MP中已经默认定义mapper映射文件的位置如下图所示，也就是标准项目中mapper文件应该存放的位置。** 所以我们可以直接不做此配置，MP已经帮我配置好了

剩下与mybatis一样的步骤，因为MP在mybatis的基础上只做增强不做改变。   
![[file-20260107162454178.png]]
![[file-20260107162447368.png]]